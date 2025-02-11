Beginner

Advanced

Kafka Producers Advanced
========================

Deep dive into Kafka producers

* * *

When you launch a Kafka producer application in Java, the application logs out the Kafka producer configurations that are being used. This includes the configurations that we have set programmatically, e.g. `bootstrap.servers`, `key.serializer`, and `value.serializer`.

The entire list is shown below.

```
[main] INFO org.apache.kafka.clients.producer.ProducerConfig - ProducerConfig values: 
	acks = all
	batch.size = 32768
	bootstrap.servers = [127.0.0.1:9092]
	buffer.memory = 33554432
	client.id = 
	compression.type = snappy
	connections.max.idle.ms = 540000
	enable.idempotence = true
	interceptor.classes = []
	key.serializer = class org.apache.kafka.common.serialization.StringSerializer
	linger.ms = 20
	max.block.ms = 60000
	max.in.flight.requests.per.connection = 5
	max.request.size = 1048576
	metadata.max.age.ms = 300000
	metric.reporters = []
	metrics.num.samples = 2
	metrics.recording.level = INFO
	metrics.sample.window.ms = 30000
	partitioner.class = class org.apache.kafka.clients.producer.internals.DefaultPartitioner
	receive.buffer.bytes = 32768
	reconnect.backoff.max.ms = 1000
	reconnect.backoff.ms = 50
	request.timeout.ms = 30000
	retries = 2147483647
	retry.backoff.ms = 100
	sasl.client.callback.handler.class = null
	sasl.jaas.config = null
	sasl.kerberos.kinit.cmd = /usr/bin/kinit
	sasl.kerberos.min.time.before.relogin = 60000
	sasl.kerberos.service.name = null
	sasl.kerberos.ticket.renew.jitter = 0.05
	sasl.kerberos.ticket.renew.window.factor = 0.8
	sasl.login.callback.handler.class = null
	sasl.login.class = null
	sasl.login.refresh.buffer.seconds = 300
	sasl.login.refresh.min.period.seconds = 60
	sasl.login.refresh.window.factor = 0.8
	sasl.login.refresh.window.jitter = 0.05
	sasl.mechanism = GSSAPI
	security.protocol = PLAINTEXT
	send.buffer.bytes = 131072
	ssl.cipher.suites = null
	ssl.enabled.protocols = [TLSv1.2, TLSv1.1, TLSv1]
	ssl.endpoint.identification.algorithm = https
	ssl.key.password = null
	ssl.keymanager.algorithm = SunX509
	ssl.keystore.location = null
	ssl.keystore.password = null
	ssl.keystore.type = JKS
	ssl.protocol = TLS
	ssl.provider = null
	ssl.secure.random.implementation = null
	ssl.trustmanager.algorithm = PKIX
	ssl.truststore.location = null
	ssl.truststore.password = null
	ssl.truststore.type = JKS
	transaction.timeout.ms = 60000
	transactional.id = null
	value.serializer = class org.apache.kafka.common.serialization.StringSerializer

```

All of these parameters are discussed in the [Kafka Producer Documentation](https://kafka.apache.org/documentation/#producerconfigs).

In this section, we will discuss some of the ones we judge are the most important:

*   [Kafka Producer Acks Deep Dive](https://github.com/AbdoMusk/Apache-Kafka/blob/main/5-%20Kafka%20Advanced%20Concepts/2-%20Kafka%20Producers%20Advanced/1-%20Kafka%20Producer%20Acks%20Deep%20Dive.md)
    
*   [Kafka Producer Retries](https://github.com/AbdoMusk/Apache-Kafka/blob/main/5-%20Kafka%20Advanced%20Concepts/2-%20Kafka%20Producers%20Advanced/2-%20Kafka%20Producer%20Retries.md)
    
*   [Idempotent Kafka Producer](https://github.com/AbdoMusk/Apache-Kafka/blob/main/5-%20Kafka%20Advanced%20Concepts/2-%20Kafka%20Producers%20Advanced/3-%20Idempotent%20Kafka%20Producer.md)
    
*   [Kafka Message Compression](https://github.com/AbdoMusk/Apache-Kafka/blob/main/5-%20Kafka%20Advanced%20Concepts/2-%20Kafka%20Producers%20Advanced/4-%20Kafka%20Message%20Compression.md)
    
*   [Kafka Producer Batching](https://github.com/AbdoMusk/Apache-Kafka/blob/main/5-%20Kafka%20Advanced%20Concepts/2-%20Kafka%20Producers%20Advanced/5-%20Kafka%20Producer%20Batching.md)
    
*   [Other Advanced Kafka Producer Configurations](https://github.com/AbdoMusk/Apache-Kafka/blob/main/5-%20Kafka%20Advanced%20Concepts/2-%20Kafka%20Producers%20Advanced/7-%20Other%20Advanced%20Kafka%20Producer%20Configurations.md)
    

**Happy learning!**