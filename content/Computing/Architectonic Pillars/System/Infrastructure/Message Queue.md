| References 
1. https://app.excalidraw.com/l/56zGeHiLyKZ/47SJUcqLNbg
###### Why MQ
1. Failures are isolated 
2. Reduce latency for user doing heavy work 
3. Improve fault tolerance 
4. Handle bursty traffic by buffering requests 

###### When to use a queue 
1. Aysnc Work - user does not need results now 
2. Bursty - absorb spikes without dropping requests. 
3. Decoupling 
	1. scale services and fail independently 
	2. Producers and consumers have different **scaling / hardware** needs (e.g., lightweight upload service vs GPU-heavy workers).
4. Reliability - queue hold messages, can't afford to lose work 
5. Note: Avoid adding a queue into truly synchronous, tight-latency paths (e.g., sub-500 ms) because it almost guarantees SLA violations.
###### What is the message Queue
1. Message queues decouple producers and consumers with a buffer so you can do expensive work
	1. asynchronously
	2. absorb spikes and
	3. improve reliability
	4. at the cost of added complexity and latency
2. It is a buffer that sits b/w consumer and the producer.
3. ![[Pasted image 20260302135349.png]]
4. Producers/Consumers are decoupled engendering scaling of producers/consumers. 


###### Delivery Mechanics and guarantees 
1. Does Messages from Queue get deleted, once it is sent to producer?
	1. No, Until consumer sends a ack message to the queue, queue do not purge it. 
2. ![[Pasted image 20260302135621.png]]
3. Delivery guarantees:
	1. At-least-Once
		1. May result in duplication, with consumer must be idempotent. 
	2. At-most-once 
		1. Fire and forget 
		2. message may not arrive 
	3. exactly-once 
		1. every message processed exactly one time 

###### How queues deal with competition from consumer to process the message?
1. **Visibility timeout** : Amazon's SQS - 30 sec configured time limit where  once a message picked up by a consumer, remains unavailable to another consumer.
2. RabbitMQ
3. **Partition-to-consumer mapping** : [[Apache Kafka]] assigning each partition to exactly one consumer in a consumer group.
4. **Prefetch limits (plus ack timeouts)**: RabbitMQ, which uses channel-level prefetch settings to control how many unacked messages a consumer can hold.

###### How queue handles increasing throughput 
1. Split a queue into multiple partition which consumers can process in parallel.  
2. ![[Pasted image 20260302141317.png]]
3. Partition key  
	1. **Ordering** (same key -> same partition -> ordered)
	2. **Load Distribution** (avoid hot partitions)
	3. Hot partition is one partition that receives far more messages than others, so the consumer(s) handling it become the bottleneck while others sit mostly idle.
	4. Partition using a partition key that is evenly distributed to avoid hot partition. 
	5. Tradeoff : Sometimes you accept **slightly uneven distribution for strict per‑entity ordering**, sometimes you favor distribution when ordering is less critical.


###### What happens when your producers outpace consumers
1. Monitor the queue depth 
2. autoscale consumers
3. add more partitions to queue 
4. apply back pressure to producers (reject/slow requests, ask clients to retry).

###### What happens when a message is a Poisoned Message
1. Poisoned Message, that may consume all the resources in its retries. 
2. Solution for this is having configuring max retries. 
3. Post max tries, move the message to DLQ
4. ![[Pasted image 20260302142629.png]]


###### What happens when queue goes down (Fault Tolerance and Durability)
1. [[Apache Kafka]] persist to disk and replicate across brokers, allowing replay of messages for recovery or reprocessing with new consumer logic.
2. So, a replica replaces the broken queue. 

###### Edge Cases 
1. When a message queue process the input but fails before sending the ack message. e.g. Charge an account $10, if fails, another consumer will do the same and account will be charged twice. 