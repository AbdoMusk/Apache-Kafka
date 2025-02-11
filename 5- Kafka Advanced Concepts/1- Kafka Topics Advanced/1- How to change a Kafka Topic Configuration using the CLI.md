How to change a Kafka Topic Configuration using the CLI?
========================================================

What are the default configurations of a Kafka topic, and how can we change it?

* * *

Broker configurations impact how topic behave in Kafka. You can find all broker configurations in the [Kafka documentation](https://kafka.apache.org/documentation/#brokerconfigs), and you may choose while running Kafka in production to use the defaults or set your own values.

The parameters you will set will impact topic performance and behavior (we discuss these below), and the defaults are usually fine, but some topics may need different values than the defaults, for example for the following values:

*   Replication Factor
    
*   Number of Partitions
    
*   Message size
    
*   Compression level
    
*   Log Cleanup Policy
    
*   Min Insync Replicas
    

The complete list of topic configurations can be found at: [https://kafka.apache.org/documentation/#topicconfigs](https://kafka.apache.org/documentation/#topicconfigs)

And we discuss some important topic configurations in [the next page](/kafka/kafka-topic-configuration-unclean-leader-election/).

* * *

Overriding Kafka Topic Configuration Defaults
---------------------------------------------

[](#Overriding-Kafka-Topic-Configuration-Defaults-0)

> [!TIP]
> **kafka-configs**
>
> kafka-configs is the tool that allows you to set configurations for specific topics

Let's see how to change the configuration `min.insync.replicas` of a topic using the CLI tool `kafka-configs`. The default value of this configuration at the broker level is `1`. As [we mentioned before](/kafka/kafka-topic-replication/), a typical value for this configuration is the **replication factor minus 1.** This means with a replication factor of 3, `min.insync.replicas` should be 2.

> [!TIP]
> **CLI Extensions**
>
> Use CLI commands with appropriate extensions for your platform, e.g., `kafka-configs.bat` for windows, `kafka-configs.sh` for Linux

Before running Kafka CLIs make sure that you have [started Kafka](/kafka/starting-kafka/) successfully.

First, create a topic named configured-topic with 3 partitions and a replication factor of 1, using Kafka topics CLI, `kafka-topics`

```
kafka-topics --bootstrap-server localhost:9092 --create --topic configured-topic --partitions 3 --replication-factor 1
```

Describe the topic to check if there are any configuration override set for this topic.

```
kafka-topics --bootstrap-server localhost:9092 --describe --topic configured-topic
```

```
Topic: configured-topic	TopicId: CDU7SBxBQ1mzJGnuH68-cQ	PartitionCount: 3	ReplicationFactor: 1	Configs:
	Topic: configured-topic	Partition: 0	Leader: 2	Replicas: 2	Isr: 2
	Topic: configured-topic	Partition: 1	Leader: 3	Replicas: 3	Isr: 3
	Topic: configured-topic	Partition: 2	Leader: 1	Replicas: 1	Isr: 1
```

There is no configuration override set.

Set the `min.insync.replicas` value for the topic `configured-topic` to 2

```
kafka-configs --bootstrap-server localhost:9092 --alter --entity-type topics --entity-name configured-topic --add-config min.insync.replicas=2
```

And describe the topic again

```
kafka-topics --bootstrap-server localhost:9092 --describe --topic configured-topic
Topic: configured-topic	TopicId: CDU7SBxBQ1mzJGnuH68-cQ	PartitionCount: 3	ReplicationFactor: 1	Configs: min.insync.replicas=2
	Topic: configured-topic	Partition: 0	Leader: 2	Replicas: 2	Isr: 2
	Topic: configured-topic	Partition: 1	Leader: 3	Replicas: 3	Isr: 3
	Topic: configured-topic	Partition: 2	Leader: 1	Replicas: 1	Isr: 1
```

Now, you can see there is a topic configuration override set (at the right side of the output) - `min.insync.replicas=2`.

You can delete the configuration override by passing `--delete-config` in place of the `--add-config` flag.

```
kafka-configs --bootstrap-server localhost:9092 --alter --entity-type topics --entity-name configured-topic --delete-config min.insync.replicas
```

Describe the topic to make sure the configuration override has been removed.