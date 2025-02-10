# What is Apache Kafka?

Learn about Apache Kafka and its ecosystem in 20 minutes.

When it comes to data event streaming, Apache Kafka is the de facto standard. It is an open-source distributed system consisting of servers and clients. Apache Kafka is used primarily to build real-time data streaming pipelines.

Apache Kafka is used by thousands of the world's leading organizations for high-performance data pipelines, streaming analytics, data integration and many other vital applications.

In this 3 part introductory series, you will learn:

* What Apache Kafka is and where it came from
* What the main components of Apache Kafka are
* What the Apache Kafka ecosystem is

By the end, you will confidently understand Apache Kafka and its place in the data streaming world.

> Looking for a headstart on Apache Kafka? The Conduktor Platform is the ultimate way to start learning Apache Kafka. Try it now for free and find out how easy Kafka can be.

## Part 1: Data Integration Challenges

### Context 

> A typical organization has multiple sources of data with disparate data formats. Data integration involves combining data from these multiple sources into one unified view of their business.

A typical business collects data through a variety of applications, e.g., accounting, billing, CRM, websites, etc. Each of these applications have their own processes for data input and update. In order to get a unified view of their business, engineers have to develop bespoke integrations between these different applications.

These direct integrations can result in a complicated solution as shown below.

![Data Integration Challenges](./static/imgs/What_is_Apache_Kafka_Part_1_-_Data_Integration_Challenges.webp)

Apache Kafka helps to solve many of the challenges associated with the integration of data from multiple different systems. This diagram shows how complex the flow of data can be when systems are not decoupled.

Each integration comes with difficulties around:

* Protocol – how the data is transported (TCP, HTTP, REST, FTP, JDBC...)
* Data format – how the data is parsed (Binary, CSV, JSON, Avro...)
* Data schema & evolution – how the data is shaped and may change
