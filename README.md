# kafka-note

##### Consumer side
fetch.message.max.bytes - this will determine the largest size of a message that can be fetched by the consumer.
##### Broker side 
replica.fetch.max.bytes - this will allow for the replicas in the brokers to send messages within the cluster and make sure the messages are replicated correctly. If this is too small, then the message will never be replicated, and therefore, the consumer will never see the message because the message will never be committed (fully replicated).
##### Broker side 
message.max.bytes - this is the largest size of the message that can be received by the broker from a producer.
Broker side (per topic): max.message.bytes - this is the largest size of the message the broker will allow to be appended to the topic. This size is validated pre-compression. (Defaults to broker's message.max.bytes.)


https://stackoverflow.com/questions/21020347/how-can-i-send-large-messages-with-kafka-over-15mb


# Features and Limitations of using Kafka for Large Messages
Originally, Kafka was not built for processing large messages and files. This does not mean that you cannot do it!

Kafka limits the max size of messages. The default value of the broker configuration' ' message.max.bytes' is 1MB.

# Why does Kafka limit the message size by default?

Different sizing, configuration, and tuning required for large message handling compared to a mission-critical real-time cluster with low latency.
Large messages increase the memory pressure on the broker JVM.
Large messages are expensive to handle and could slow down the brokers.
A reasonable message size limit can meet the requirements of most use cases.
Good workarounds exist if you need to handle large messages.
Most cloud offerings don't allow large messages.
There are noticeable performance impacts from increasing the allowable message size.

Hence, understand all alternatives discussed below before sending messages >1Mb through your Kafka cluster. Depending on your SLAs for uptime and latency, a separate Kafka cluster should be considered for processing large messages.

Having said this, I have seen customers processing messages far bigger than 10Mb with Kafka. It is valid to evaluate Kafka for processing large messages instead of using another tool for that (often in conjunction with Kafka).

LinkedIn talked a long time ago about the pros and cons of two different approaches: Using 'Kafka only' vs. 'Kafka in conjunction with another data storage'. Especially outside the public cloud, most enterprises cannot simply use an S3 object store for big data. Therefore, the question comes up if one system (Kafka) is good enough, or if you should invest in two systems (Kafka and external storage).

Let's take a look at the trade-offs for using Kafka for large messages.

# Kafka for Large Messages – Alternatives and Trade-Offs
There is no single best solution. The decision on how to handle large messages with Kafka depends on your use cases, SLAs, and already existing infrastructure.

The following three available alternatives exist to handle large messages with Kafka:

Reference-based messaging in Kafka and external storage
In-line large message support in Kafka without external storage
In-line large message support and tiered storage in Kafka
Here are the characteristics and pros/cons of each approach (this is an extension from a LinkedIn presentation in 2016):

Apache Kafka for large message payloads and files - Alternatives and Trade-offs

Also, don't underestimate the power of compression for large messages. Some big files like CSV or XML can reduce its size significantly just by setting the compression parameter to use GZIP, Snappy, or LZ4.

Even a 1GB file could be sent via Kafka, but this is undoubtedly not what Kafka was designed for. In both the client and the broker, a 1GB chunk of memory will need to be allocated in JVM for every 1GB message. Hence, in most cases, for really large files, it is better to externalize them into an object store and use Kafka just for the metadata. 

You need to define what is 'a large message' by yourself and when to use which of the design patterns discussed in this blog post. That's why I am writing this up here... :-)

The following sections explore these alternatives in more detail. Before we start, let's explain the general concept of Tiered Storage for Kafka mentioned in the above table. Many readers might not be aware of this yet.

https://dzone.com/articles/processing-large-messages-with-apache-kafka
