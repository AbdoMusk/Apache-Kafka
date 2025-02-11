Java Consumer in Threads
========================

A quick lesson for advanced users on running Kafka Consumers in a separate thread

* * *

Running a Java Consumer in a separate thread allows you to perform other tasks in the main thread.

This is only recommended if you know about multi-threaded programming, so we will keep this page brief. Most use cases will not require you to use threads.

As multi-threading can be complicated for those who have not used it before, we will leave the advanced users the responsibility of analyzing the code below.

Consumer in Threads Sample code
-------------------------------

[](#Consumer-in-Threads-Sample-code-0)

```
package io.conduktor.demos.kafka;

import org.apache.kafka.clients.consumer.*;
import org.apache.kafka.common.errors.WakeupException;
import org.apache.kafka.common.serialization.StringDeserializer;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.time.Duration;
import java.util.Collections;
import java.util.Date;
import java.util.Properties;
import java.util.concurrent.CountDownLatch;

public class ConsumerDemoThreads {

    public static void main(String[] args)  {
        ConsumerDemoWorker consumerDemoWorker = new ConsumerDemoWorker();
        new Thread(consumerDemoWorker).start();
        Runtime.getRuntime().addShutdownHook(new Thread(new ConsumerDemoCloser(consumerDemoWorker)));
    }

    private static class ConsumerDemoWorker implements Runnable {

        private static final Logger log = LoggerFactory.getLogger(ConsumerDemoWorker.class);

        private CountDownLatch countDownLatch;
        private Consumer<String, String> consumer;

        @Override
        public void run() {
            countDownLatch = new CountDownLatch(1);
            final Properties properties = new Properties();

            properties.setProperty(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
            properties.setProperty(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
            properties.setProperty(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
            properties.setProperty(ConsumerConfig.GROUP_ID_CONFIG, "my-sixth-application");
            properties.setProperty(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

            consumer = new KafkaConsumer<>(properties);
            consumer.subscribe(Collections.singleton("demo_java"));

            final Duration pollTimeout = Duration.ofMillis(100);

            try {
                while (true) {
                    final ConsumerRecords<String, String> consumerRecords = consumer.poll(pollTimeout);
                    for (final ConsumerRecord<String, String> consumerRecord : consumerRecords) {
                        log.info("Getting consumer record key: '" + consumerRecord.key() + "', value: '" + consumerRecord.value() + "', partition: " + consumerRecord.partition() + " and offset: " + consumerRecord.offset() + " at " + new Date(consumerRecord.timestamp()));
                    }
                }
            } catch (WakeupException e) {
                log.info("Consumer poll woke up");
            } finally {
                consumer.close();
                countDownLatch.countDown();
            }
        }

        void shutdown() throws InterruptedException {
            consumer.wakeup();
            countDownLatch.await();
            log.info("Consumer closed");
        }

    }

    private static class ConsumerDemoCloser implements Runnable {

        private static final Logger log = LoggerFactory.getLogger(ConsumerDemoCloser.class);

        private final ConsumerDemoWorker consumerDemoWorker;

        ConsumerDemoCloser(final ConsumerDemoWorker consumerDemoWorker) {
            this.consumerDemoWorker = consumerDemoWorker;
        }

        @Override
        public void run() {
            try {
                consumerDemoWorkerutdown();
            } catch (InterruptedException e) {
                log.error("Error shutting down consumer", e);
            }
        }
    }
}
```

Output
------

[](#Output-1)

```
[Thread-0]  Consumer poll woke up
[Thread-0]  Revoke previously assigned partitions demo_java-0
[Thread-0]  Member consumer-my-sixth-application-1-fecba584-5838-418d-83b8-c88a528bc0e5 sending LeaveGroup request to coordinator 127.0.0.1:9094 (id: 2147483644 rack: null) due to the consumer is being closed
[Thread-0]  Resetting generation due to: consumer pro-actively leaving the group
[Thread-0]  Request joining group due to: consumer pro-actively leaving the group
[Thread-0]  Metrics scheduler closed
[Thread-0]  Closing reporter org.apache.kafka.common.metrics.JmxReporter
[Thread-0]  Metrics reporters closed
[Thread-0]  App info kafka.consumer for consumer-my-sixth-application-1 unregistered
[Thread-1]  Consumer closed
```

---
Next: [Kafka Advanced Concepts](https://github.com/AbdoMusk/Apache-Kafka/tree/main/5-%20Kafka%20Advanced%20Concepts)