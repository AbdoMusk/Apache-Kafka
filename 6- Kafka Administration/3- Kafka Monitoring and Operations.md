Kafka Monitoring and Operations
===============================

How can we monitor a Kafka cluster? What are the tools available to operate a Kafka cluster?

* * *

Monitoring is a key aspect of running Apache Kafka properly. In this section, we'll learn how Kafka exposes metrics, and some of the key metrics to monitor. We'll also look at some of the operations that one should master when running a cluster.

* * *

Kafka Monitoring
----------------

[](#Kafka-Monitoring-0)

Kafka runs on the JVM (Java Virtual Machine). All of the metrics exposed by Kafka can be accessed via the Java Management Extensions (JMX) interface. The easiest way to use them in an external monitoring system is to use a collection agent provided by your monitoring system and attach it to the Kafka process.

The collection agent can then be scrapped for metrics by various monitoring toolkits. Common places to host the Kafka metrics are:

*   ELK (ElasticSearch + Kibana)
    
*   Datadog
    
*   NewRelic
    
*   Confluent Control Centre
    
*   Prometheus
    

These monitoring systems have alerting capabilities that can be set up based on the metrics.

Today most monitoring systems provide good documentation on how to integrate monitoring for Apache Kafka with the monitoring tool itself.

#### Kafka Metrics to monitor

No matter how you collect metrics from Kafka, you should have a way to also monitor the overall health of the application process via key metrics. There are many metrics exposed by different Kafka components providing information about nearly every function of that component. We'll review the most common ones that provide the information needed to run Kafka on a daily basis.

*   **Under Replicated Partitions**: This measurement, provided on each broker in a cluster, gives a count of the number of partitions for which the broker is the leader replica, where the follower replicas are not caught up. A high number may indicate a high load on the system.
    
*   **OfflinePartitionsCount**: Number of partitions that are offline (which is not good for availability)
    
*   **Request Handlers**: This measurement provides information about the utilization of threads for IO, network, etc. for each Kafka broker.
    
*   **Request timing**: This metrics provides information about how long it takes to reply to requests. Lower is better, as latency will be improved.
    
*   **Active Controller Count**: there should only be one controller in your cluster, so the value of this should always be 1.
    
*   and many more!
    

Metrics are exposed using JMX in Kafka, although java agents or various vendors can help collect or expose metrics on different ports (for example Prometheus, etc...)

Additionally, on top of broker metrics, client metrics in general (Producer, Consumer, Kafka Streams, Kafka Connect...) are important to collect and monitor.

This page is meant as an introduction, and more content will soon be created towards metrics and monitoring in Apache Kafka.

#### References

There are many metrics exposed by Kafka providing information about nearly every function. To learn more about them, these references are very helpful:

*   [https://kafka.apache.org/documentation/#monitoring](https://kafka.apache.org/documentation/#monitoring)
    
*   [https://docs.confluent.io/current/kafka/monitoring.html](https://docs.confluent.io/current/kafka/monitoring.html)
    
*   [https://www.datadoghq.com/blog/monitoring-kafka-performance-metrics/](https://www.datadoghq.com/blog/monitoring-kafka-performance-metrics/)
    

* * *

Kafka Cluster Operations
------------------------

[](#Kafka-Cluster-Operations-1)

For the day-to-day operations of Kafka, there are a number of operations that one has to learn and master to be able to perform them safely. These include:

*   Rolling Restart of Brokers
    
*   Updating Configurations
    
*   Rebalancing Partitions
    
*   Increasing replication factor
    
*   Adding a Broker
    
*   Replacing a Broker
    
*   Removing a Broker
    
*   Upgrading a Kafka Cluster with zero downtime
    

It is important to remember that managing your own cluster comes with all these responsibilities and more.

---
Next: [Kafka Cluster Setup Overview](https://github.com/AbdoMusk/Apache-Kafka/blob/main/6-%20Kafka%20Administration/4-%20Kafka%20Cluster%20Setup%20Overview.md)