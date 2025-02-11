Kafka Topic Naming Conventions
==============================

What are the naming conventions for a Kafka topic?

* * *

Kafka topic can have any names you want. It is very important to choose a naming convention in your company to maintain some kind of consistency.

If you need ideas, I recommend reading this blog: [https://cnr.sh/essays/how-paint-bike-shed-kafka-topic-naming-conventions](https://cnr.sh/essays/how-paint-bike-shed-kafka-topic-naming-conventions)

Common structures adopted are hierarchical such as `<department name>.<team name>.<dataset name>.<data format>` (you can define your own).

Suffixing by <data format> can be a good way to indicate in advance how to consume a topic for example `.avro` , `.json`, `.text`, `.protobuf`, `.csv`, `.log`

Valid characters for Kafka topics are the ASCII alphanumerics, `.`, `_`, and `-` and it is better not to mix `.` and `_` to avoid metric namespace collisions:

[](javascript:void(0);)

```
org.apache.kafka.common.errors.InvalidTopicException: Topic 'test.1' collides with existing topics: test_1
```

---
Next: [Kafka Producers Advanced](https://github.com/AbdoMusk/Apache-Kafka/tree/main/5-%20Kafka%20Advanced%20Concepts/2-%20Kafka%20Producers%20Advanced)