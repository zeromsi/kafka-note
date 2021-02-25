# kafka-note

Consumer side:fetch.message.max.bytes - this will determine the largest size of a message that can be fetched by the consumer.
Broker side: replica.fetch.max.bytes - this will allow for the replicas in the brokers to send messages within the cluster and make sure the messages are replicated correctly. If this is too small, then the message will never be replicated, and therefore, the consumer will never see the message because the message will never be committed (fully replicated).
Broker side: message.max.bytes - this is the largest size of the message that can be received by the broker from a producer.
Broker side (per topic): max.message.bytes - this is the largest size of the message the broker will allow to be appended to the topic. This size is validated pre-compression. (Defaults to broker's message.max.bytes.)


https://stackoverflow.com/questions/21020347/how-can-i-send-large-messages-with-kafka-over-15mb
