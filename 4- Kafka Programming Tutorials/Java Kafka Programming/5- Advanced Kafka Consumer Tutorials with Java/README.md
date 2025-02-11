Advanced Kafka Consumer Tutorials with Java
===========================================

A series of tutorials covering more advanced Java programming tasks for Kafka Consumers

* * *

The previous code examples should be enough for 90% of your use cases.

Here, we have included advanced examples for the following use cases:

*   [Consumer Rebalance Listener](/kafka/java-consumer-rebalance-listener/): in case you're doing a manual commit of your offsets to Kafka or externally, this allows you to commit offsets when partitions are revoked from your consumer.
    
*   [Consumer Seek and Assign](/kafka/java-consumer-seek-and-assign/): if instead of using consumer groups, you want to start at specific offsets and consume only specific partitions, you will learn about the `.seek()` and `.assign()` APIs.
    
*   [Consumer in Threads](/kafka/java-consumer-in-threads/): very advanced code samples, this is for those who want to run their consumer `.poll()` loop in a separate thread.