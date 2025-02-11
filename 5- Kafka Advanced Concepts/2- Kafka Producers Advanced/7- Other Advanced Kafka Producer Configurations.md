Other Advanced Kafka Producer Configurations
============================================

What are the important Kafka producer configurations?

* * *

Advanced Producer settings
--------------------------

[](#Advanced-Producer-settings-0)

You may encounter some edge cases where these settings might help.

buffer.memory
-------------

[](#buffer.memory-1)

This sets the amount of memory the producer will use to buffer messages waiting to be sent to brokers. Consider the scenario where the producer is sending messages faster than the broker can handle. In that case, the records will remain buffered in the producer's memory. Each producer maintains a buffer memory of `32 MB,` by default. This is called the send buffer. The buffer may fill up over time and fill back down when the throughput to the broker increases.

When the buffer memory is full, the producer's `send()` method will block. This is controlled by the producer setting `max.block.ms`.

max.block.ms
------------

[](#max.block.ms-2)

This parameter controls how long the producer will block when calling `send()` and when explicitly requesting metadata via `partitionsFor()`. Those methods block when the producer’s send buffer is full or when metadata is not available. When `max.block.ms` is reached, a timeout exception is thrown. The default value is `60000`.

---
Next: [Kafka Consumers Advanced](https://github.com/AbdoMusk/Apache-Kafka/tree/main/5-%20Kafka%20Advanced%20Concepts/2-%20Kafka%20Producers%20Advanced/8-%20Kafka%20Consumers%20Advanced)