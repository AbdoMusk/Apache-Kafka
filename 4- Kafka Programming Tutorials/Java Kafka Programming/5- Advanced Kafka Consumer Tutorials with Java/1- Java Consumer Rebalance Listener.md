Java Consumer Rebalance Listener
================================

Advanced Kafka Programming Tutorial for Rebalancing Listeners in your Kafka Consumer with Java

* * *

Consumer rebalances happen for the following events:

*   Number of partitions change for any of the subscribed topics
    
*   A subscribed topic is created or deleted
    
*   An existing member of the consumer group is shutdown or fails
    
*   A new member is added to the consumer group
    

When any of these events are triggered, the provided listener will be invoked **twice**: first to indicate that the consumer's assignment has been revoked, and then again when the new assignment has been received.

In that case, we have a chance to commit offsets and some cleanup work before our partition is revoked. This includes maybe closing database connections, etc..

If we handle this case gracefully, we will not process duplicate messages through rebalances.

> [!TIP]
> **Very important**
>
> Note that rebalances will only occur during an active call to `poll(Duration)`, so callbacks will also only be invoked during that time. You do not need to worry about threading because calls happen in the same consumer thread.

* * *

When should you use a Consumer Rebalance?
-----------------------------------------

[](#When-should-you-use-a-Consumer-Rebalance?-0)

Excerpt from the [ConsumerRebalanceListener](https://kafka.apache.org/24/javadoc/index.html?org/apache/kafka/clients/consumer/ConsumerRebalanceListener.html) documentation:

One common use is saving offsets in a custom store. By saving offsets in the `onPartitionsRevoked(Collection)` call we can ensure that any time partition assignment changes the offset gets saved.

Another use is flushing out any kind of cache of intermediate results the consumer may be keeping. For example, consider a case where the consumer is subscribed to a topic containing user page views, and the goal is to count the number of page views per user for each five-minute window. Let's say the topic is partitioned by the user id so that all events for a particular user go to a single consumer instance. The consumer can keep in memory a running tally of actions per user and only flush these out to a remote data store when its cache gets too big. However if a partition is reassigned, your consumer may want to automatically trigger a flush of this cache, before the new owner takes over consumption.

Note that callbacks only serve as notification of an assignment change. They cannot be used to express acceptance of the change. Hence throwing an exception from a callback does not affect the assignment in any way, as it will be propagated all the way up to the `KafkaConsumer.poll(java.time.Duration)` call. If user captures the exception in the caller, the callback is still assumed successful and no further retries will be attempted.

* * *

Consumer Rebalance Listeners Example
------------------------------------

[](#Consumer-Rebalance-Listeners-Example-1)

### Consumer Rebalance Listener Implementation

[](#Consumer-Rebalance-Listener-Implementation-0)

In this example, we have created a `ConsumerRebalanceListener` that keeps track of how far we have been consuming in our Kafka topic partitions.

```
package io.conduktor.demos.kafka;

import org.apache.kafka.clients.consumer.ConsumerRebalanceListener;
import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.clients.consumer.OffsetAndMetadata;
import org.apache.kafka.common.TopicPartition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

public class ConsumerRebalanceListenerImpl implements ConsumerRebalanceListener {

    private static final Logger log = LoggerFactory.getLogger(ConsumerRebalanceListenerImpl.class);

    private KafkaConsumer<String, String> consumer;
    private Map<TopicPartition, OffsetAndMetadata> currentOffsets = new HashMap<>();

    public ConsumerRebalanceListenerImpl(KafkaConsumer<String, String> consumer) {
        this.consumer = consumer;
    }

    public void addOffsetToTrack(String topic, int partition, long offset){
        currentOffsets.put(
                new TopicPartition(topic, partition),
                new OffsetAndMetadata(offset + 1, null));
    }

    @Override
    public void onPartitionsRevoked(Collection<TopicPartition> partitions) {
        log.info("onPartitionsRevoked callback triggered");
        log.info("Committing offsets: " + currentOffsets);

        consumer.commitSync(currentOffsets);
    }

    @Override
    public void onPartitionsAssigned(Collection<TopicPartition> partitions) {
        log.info("onPartitionsAssigned callback triggered");
    }

    // this is used when we shut down our consumer gracefully
    public Map<TopicPartition, OffsetAndMetadata> getCurrentOffsets() {
        return currentOffsets;
    }
}
```

Important things to note:

*   we track internally in the class the offsets of how far we have consumers using `currentOffsets`
    
*   in the function `addOffsetToTrack` we make sure to increment the offset by 1 in order to commit the position properly
    
*   we use a synchronous `consumer.commitSync` call in `onPartitionsRevoked` to block until the offsets are successfully committed
    

### Using the Rebalance Listener in our Consumer code

[](#Using-the-Rebalance-Listener-in-our-Consumer-code-1)

Here is the implementation:

```
package io.conduktor.demos.kafka;

import org.apache.kafka.clients.consumer.*;
import org.apache.kafka.common.errors.WakeupException;
import org.apache.kafka.common.serialization.StringDeserializer;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.time.Duration;
import java.util.Arrays;
import java.util.Properties;

public class ConsumerDemoRebalanceListener {
    private static final Logger log = LoggerFactory.getLogger(ConsumerDemoRebalanceListener.class);

    public static void main(String[] args) {
        log.info("I am a Kafka Consumer with a Rebalance");

        String bootstrapServers = "127.0.0.1:9092";
        String groupId = "my-fifth-application";
        String topic = "demo_java";

        // create consumer configs
        Properties properties = new Properties();
        properties.setProperty(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        properties.setProperty(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.setProperty(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.setProperty(ConsumerConfig.GROUP_ID_CONFIG, groupId);
        properties.setProperty(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

        // we disable Auto Commit of offsets
        properties.setProperty(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, "false");

        // create consumer
        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(properties);

        ConsumerRebalanceListenerImpl listener = new ConsumerRebalanceListenerImpl(consumer);

        // get a reference to the current thread
        final Thread mainThread = Thread.currentThread();

        // adding the shutdown hook
        Runtime.getRuntime().addShutdownHook(new Thread() {
            public void run() {
                log.info("Detected a shutdown, let's exit by calling consumer.wakeup()...");
                consumer.wakeup();

                // join the main thread to allow the execution of the code in the main thread
                try {
                    mainThread.join();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        try {
            // subscribe consumer to our topic(s)
            consumer.subscribe(Arrays.asList(topic), listener);

            // poll for new data
            while (true) {
                ConsumerRecords<String, String> records =
                        consumer.poll(Duration.ofMillis(100));

                for (ConsumerRecord<String, String> record : records) {
                    log.info("Key: " + record.key() + ", Value: " + record.value());
                    log.info("Partition: " + record.partition() + ", Offset:" + record.offset());

                    // we track the offset we have been committed in the listener
                    listener.addOffsetToTrack(record.topic(), record.partition(), record.offset());
                }

                // We commitAsync as we have processed all data and we don't want to block until the next .poll() call
                consumer.commitAsync();
            }
        } catch (WakeupException e) {
            log.info("Wake up exception!");
            // we ignore this as this is an expected exception when closing a consumer
        } catch (Exception e) {
            log.error("Unexpected exception", e);
        } finally {
            try {
                consumer.commitSync(listener.getCurrentOffsets()); // we must commit the offsets synchronously here
            } finally {
                consumer.close();
                log.info("The consumer is now gracefully closed.");
            }
        }
    }
}
```

Important things to note:

*   We disable auto commit (otherwise we wouldn't need a rebalance listener)
    
*   on every message being successfully synchronously processed, we call `listener.addOffsetToTrack(record.topic(), record.partition(), record.offset());` which allows us to track how far we've been processing in our consumer
    
*   when we're done with a batch we call `consumer.commitAsync();` to commit offsets without blocking our consumer loop.
    
*   on the consumer shutdown, we finally call again `consumer.commitSync(listener.getCurrentOffsets());` to commit one last time based on how far we've read before closing the consumer.

---
Next: [Java Consumer Seek and Assign](https://github.com/AbdoMusk/Apache-Kafka/blob/main/4-%20Kafka%20Programming%20Tutorials/Java%20Kafka%20Programming/5-%20Advanced%20Kafka%20Consumer%20Tutorials%20with%20Java/2-%20Java%20Consumer%20Seek%20and%20Assign.md)