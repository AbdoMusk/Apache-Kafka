Kafka Message Compression
=========================

How does message compression work in Kafka?

* * *

Producers usually send data that is text-based, for example with JSON data. In this case, it is important to apply compression to the producer. **By default, producer messages are sent uncompressed.**

Apache Kafka Message Compression
--------------------------------

[](#Apache-Kafka-Message-Compression-0)

Kafka supports two types of compression: producer-side and broker-side.

*   **Compression enabled producer-side** doesn’t require any configuration change in the brokers or in the consumers. Producers may choose to compress messages with the `compression.type` setting.
    
    *   Options are `none`, `gzip`, `lz4`, `snappy`, and `zstd`
        
    *   Compression is performed by the producer client, if enabled
        
    *   This is particularly efficient if the producer batches messages together (high throughput)
        
*   **Compression enabled broker-side (topic-level)** with the setting `compression.type`:
    
    *   if the compression codec of the destination topic is set to `compression.type=producer`, the broker takes the compressed batch from the client and writes it directly to the topic’s log file without recompressing the data.
        
    *   by default the topic compression is defined as `compression.type=producer`
        
    *   If the topic has its own compression setting (for example `lz4`) and the value matches the producer setting, the message batch is written as is to the log file, otherwise, the broker will decompress and recompress the messages into its configured format.
        

Producer-Level Message Compression in Kafka
-------------------------------------------

[](#Producer-Level-Message-Compression-in-Kafka-1)

Producers group messages in a batch before sending. This is done to save network trips. If the producer is sending compressed messages, all the messages in a single producer batch are compressed together and sent as the "value" of a "wrapper message". Compression is more effective the bigger the batch of messages being sent to Kafka!

![Diagram showing the process behind producer-level message compression in Apache Kafka.](../../static/images/Adv_Kafka_Message_Compression_1.webp "Apache Kafka Producer-Level Message Compression")

Message Compression

To increase the efficiency of compression, optimize your [Kafka Producer Batching](https://github.com/AbdoMusk/Apache-Kafka/blob/main/5-%20Kafka%20Advanced%20Concepts/2-%20Kafka%20Producers%20Advanced/5-%20Kafka%20Producer%20Batching.md).

By enabling compression, you reduce network utilization and storage, which is often a bottleneck when sending messages to Kafka. The compressed batch has the following advantage:

*   Much smaller producer request size (compression ratio up to 4x!)
    
*   Faster to transfer data over the network => less latency
    
*   Better throughput
    
*   Better disk utilization in Kafka (stored messages on disk are smaller)
    

Compression, however, has a small overhead on CPU resources as it involves compression and decompression.

*   Producers must commit some CPU cycles to compression
    
*   Consumers must commit some CPU cycles to decompression
    

> [!TIP]
> **Speed vs Compression**
>
> Consider testing snappy or lz4 for optimal speed / compression ratio

Always use compression in production and especially if you have high throughput.

Note: `zstd` compression was introduced after Kafka 2.1.

---
Next: [Kafka Producer Batching](https://github.com/AbdoMusk/Apache-Kafka/blob/main/5-%20Kafka%20Advanced%20Concepts/2-%20Kafka%20Producers%20Advanced/5-%20Kafka%20Producer%20Batching.md)