Advanced Kafka Consumer Tutorials with Java
===========================================

A series of tutorials covering more advanced Java programming tasks for Kafka Consumers

* * *

The previous code examples should be enough for 90% of your use cases.

Here, we have included advanced examples for the following use cases:

*   [Consumer Rebalance Listener](https://github.com/AbdoMusk/Apache-Kafka/blob/main/4-%20Kafka%20Programming%20Tutorials/Java%20Kafka%20Programming/5-%20Advanced%20Kafka%20Consumer%20Tutorials%20with%20Java/1-%20Java%20Consumer%20Rebalance%20Listener.md): in case you're doing a manual commit of your offsets to Kafka or externally, this allows you to commit offsets when partitions are revoked from your consumer.
    
*   [Consumer Seek and Assign](https://github.com/AbdoMusk/Apache-Kafka/blob/main/4-%20Kafka%20Programming%20Tutorials/Java%20Kafka%20Programming/5-%20Advanced%20Kafka%20Consumer%20Tutorials%20with%20Java/2-%20Java%20Consumer%20Seek%20and%20Assign.md): if instead of using consumer groups, you want to start at specific offsets and consume only specific partitions, you will learn about the `.seek()` and `.assign()` APIs.
    
*   [Consumer in Threads](https://github.com/AbdoMusk/Apache-Kafka/blob/main/4-%20Kafka%20Programming%20Tutorials/Java%20Kafka%20Programming/5-%20Advanced%20Kafka%20Consumer%20Tutorials%20with%20Java/3-%20Java%20Consumer%20in%20Threads.md): very advanced code samples, this is for those who want to run their consumer `.poll()` loop in a separate thread.