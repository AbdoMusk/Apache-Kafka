Kafka Broker & Client Upgrades
==============================

How to upgrade Kafka Brokers and Kafka Clients?

* * *

Kafka Client Version Upgrades
-----------------------------

[](#Kafka-Client-Version-Upgrades-0)

As of Kafka 0.10.2 (introduced in July 2017), Kafka clients and Kafka Brokers have **bi-directional compatibility** because API calls are now versioned. In other words, newer clients can talk to older servers, and older clients can talk to newer servers.

This means:

*   An OLDER client, e.g., 2.1 can talk to a NEWER broker, e.g., 3.0
    
*   A NEWER client, e.g., 3.0 can talk to an OLDER broker, e.g., 2.1
    

This allows Kafka administrators and developers to upgrade either clients or servers at any time without experiencing any downtime.

> [!TIP]
> **Bottom Line**
>
> Always use the latest client library version if you can

You can read more about client bi-directional compatibility here: [https://www.confluent.io/blog/upgrading-apache-kafka-clients-just-got-easier/](https://www.confluent.io/blog/upgrading-apache-kafka-clients-just-got-easier/)

* * *

Kafka Broker Version Upgrades
-----------------------------

[](#Kafka-Broker-Version-Upgrades-1)

Upgrading Kafka brokers is by far the most important operation that we have to do on our cluster. It is recommended to upgrade very often in order to keep up with bug fixes, new features, and performance improvements. The more we wait to upgrade a broker, the more painful it will be as there will be many versions jump.

Instructions on how to upgrade Kafka are always [outlined in the documentation](https://kafka.apache.org/documentation/#upgrade). Regardless, the steps are usually pretty similar from version to version, as you'll see below.

### How to upgrade Kafka brokers without downtime?

[](#How-to-upgrade-Kafka-brokers-without-downtime?-0)

It is fairly simple to upgrade Kafka. There are five steps to upgrade with a rolling restart required at each step. In this example we are giving the steps to upgrade from version Kafka v2.1 to v3.0.

1.  Set inter broker and log version to current Kafka version. If you are using 2.1 now, just put the following broker properties: `inter.broker.protocol.version=2.1` and `log.message.format.version=2.1`
    
2.  One broker at a time: stop the broker, upgrade the binaries to 3.0, and start the broker. Wait for the cluster to stabilize (all the partitions are in-sync replicas (ISR).
    
3.  At this stage, all your brokers are running the newer binaries of 3.0, but running with `inter.broker.protocol.version=2.1` and `log.message.format.version=2.1`
    
4.  Ensure the cluster is stable in this form. You still have the option to downgrade here if you want to because the log message format has not been upgraded yet.
    
5.  One broker at a time: stop the broker, change the inter broker protocol version for that broker with `inter.broker.protocol.version=3.0` and start that broker. Wait for the cluster to stabilize (all the partitions are in-sync replicas (ISR).
    
6.  At the end of this stage, all the brokers are running with the new inter broker protocol version, but they are not using the new log message format yet.
    
7.  Upgrade Kafka clients if documentation specifies it (all or most of them to avoid up and down conversion - which can affect performance)
    
8.  One broker at a time: stop the broker, upgrade message protocol version `log.message.format.version=3.0` and start the broker. Wait for the cluster to stabilize (all the partitions are in-sync replicas (ISR).
    
9.  At this stage, all brokers are upgraded to the latest binary, are running the latest inter-broker protocol and are using the latest log message format version. Congratulations!
    

Refer to the documentation at: [https://kafka.apache.org/documentation/#upgrade](https://kafka.apache.org/documentation/#upgrade)
