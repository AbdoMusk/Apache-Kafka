Kafka SDK List
==============

Learn how to choose the right client libraries for Kafka and explore the Kafka Client Libraries SDK list

* * *

Apache Kafka only officially provides an SDK for the Java language.

**For the other languages**, these are community based implementations. A list of recommended SDK has been compiled below to save you a little bit of time.

> [!TIP]
> **Feedback?**
>
> Something looks wrong? Send us an email at feedback \[at\] conduktor \[dot\] io

* * *

How to choose a Kafka client library?
-------------------------------------

[](#How-to-choose-a-Kafka-client-library?-0)

Usually most of the libraries will have support for the producer and consumer API. Decisions points come around the following:

*   **Pure implementation or** [**librdkafka**](https://github.com/edenhill/librdkafka) **based**: librdkafka is a C/C++ library that implements the Kafka API and many libraries are built as a wrapper around it. This may be acceptable to you, or not. The main issues can arise when you build and deploy your software.
    
*   **Does it support your security mechanism?** Most libraries will for sure support PLAINTEXT, but ensure it supports the security mechanism you need for your Kafka deployment: SSL and SASL (SASL/SCRAM, SASL/OAUTHBEARER, SASL/GSSAPI, SASL/PLAIN, or even external SASL mechanisms such as SASL IAM for MSK)
    
*   **Does it support the** **Confluent Schema Registry?** The Confluent Schema Registry is a common implementation in a Kafka deployment, and making sure your libraries have the proper serializers and deserializers for it is very important. Usually, Confluent libraries do a good job at integrating with the Confluent Schema Registry. If it supports the Schema Registry, does it support Avro, JSON, or Protobuf? (based on your decisions)
    
*   **Does it support the performance you need?** This is usually the case, but make sure to do performance testing before going with a full blown implementation. This may save you some time.
    

* * *

Kafka Client Libraries SDK List
-------------------------------

[](#Kafka-Client-Libraries-SDK-List-1)

Here you can find a list of libraries that has been compiled to ensure you have some pointers on when to start with your implementation:

*   Java
    
    *   [The official client library](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients): low-level client
        
    *   [The official Kafka Streams client library](https://mvnrepository.com/artifact/org.apache.kafka/kafka-streams): to create your Kafka Streams application
        
    *   [Kafka for Spring Boot](https://spring.io/projects/spring-kafka): applies Spring concepts to Kafka development
        
    *   [Spring Cloud Stream](https://spring.io/projects/spring-cloud-stream): bindings for Kafka Stream
        
    *   [Akka Streams](https://doc.akka.io/docs/akka/current/stream/index.html) & [Alpakka Kafka](https://doc.akka.io/docs/alpakka-kafka/current/home.html)
        
*   Scala
    
    *   [FS2 Kafka](https://fd4s.github.io/fs2-kafka/): Functional Kafka Producer and Consumer
        
    *   [ZIO Kafka](https://github.com/zio/zio-kafka): Kafka Client for ZIO
        
    *   [Kafka Streams Scala](https://mvnrepository.com/artifact/org.apache.kafka/kafka-streams-scala): the official kafka-streams library has Scala [since Kafka 2.0](https://issues.apache.org/jira/browse/KAFKA-6670)
        
    *   [Alpakka Kafka](https://doc.akka.io/docs/alpakka-kafka/current/home.html)
        
*   C/C++
    
    *   [librdkafka](https://github.com/edenhill/librdkafka): low-level implementation of the Kafka client, many other higher level language libraries are a wrapper around this library. Supports all kind of security, and [most KIPs](https://github.com/edenhill/librdkafka/blob/master/INTRODUCTION.md#supported-kips)
        
    *   [CPP Kafka](https://github.com/mfontanini/cppkafka): based on librdkafka
        
    *   [Modern CPP Kafka](https://github.com/morganstanley/modern-cpp-kafka): based on librdkafka
        
*   Golang
    
    *   [Confluent Kafka Go](https://github.com/confluentinc/confluent-kafka-go): wrapper of librdkafka, currently [no Schema Registry support](https://github.com/confluentinc/confluent-kafka-go/issues/69)
        
    *   [Schema Registry Client for Go](https://github.com/riferrei/srclient): compatible with Confluent Kafka Go
        
    *   [Segment's Kafka Go](https://github.com/segmentio/kafka-go): pure Go implementation of the Kafka Client, good support
        
    *   [Franz Go](https://github.com/twmb/franz-go): Pure go implementation, supports most KIPs
        
    *   others if you're curious: [Sarama](https://github.com/Shopify/sarama), [Goka](https://github.com/lovoo/goka)
        
*   Python
    
    *   [Confluent Kafka Python](https://github.com/confluentinc/confluent-kafka-python): based on librdkafka, includes an Admin client, Avro support with the Confluent Schema Registry
        
    *   [Kafka Python](https://github.com/dpkp/kafka-python): not active, use at your own risk
        
*   Javascript / Node.js
    
    *   [KafkaJS](https://kafka.js.org/): no external dependencies, good performance, support for [Schema Registry](https://github.com/kafkajs/confluent-schema-registry)
        
    *   [Blizzard Node rdkafka](https://github.com/Blizzard/node-rdkafka): Node.js wrapper for librdkafka, low maintenance activity
        
*   .NET / C#
    
    *   [Confluent Kafka DotNet](https://github.com/confluentinc/confluent-kafka-dotnet): wrapper around librdkafka, full Schema Registry support (Avro, JSON, Protobuf)
        
*   Rust
    
    *   [Rust rdkafka](https://github.com/fede1024/rust-rdkafka): Rust wrapper for librdkafka, good performance
        
    *   [Rust Schema Registry Converter](https://github.com/gklijs/schema_registry_converter): compatible with Rust rdkafka
        
    *   [Kafka Rust](https://github.com/kafka-rust/kafka-rust): pure Rust implementation, low maintenance activity
        
*   REST API
    
    *   [Confluent REST Proxy](https://github.com/confluentinc/kafka-rest): REST API for Kafka. Deploy with a reverse proxy sidecar to implement the security mechanism you need. Historically needed, but now that client library support for Kafka is quite good, you migh not have a use case for it.
        
*   Kotlin
    
    *   simply use the standard Java library
        
*   Haskell
    
    *   [HW Kafka Client](https://github.com/haskell-works/hw-kafka-client): based on librdkafka
        
    *   [HW Kafka Avro](https://github.com/haskell-works/hw-kafka-avro): support for Schema Registry
        
*   Ruby
    
    *   [rdkafka-ruby](https://github.com/appsignal/rdkafka-ruby): based on librdkafka
        
    *   [Ruby Kafka](https://github.com/zendesk/ruby-kafka): limited support for new API