
References 
1. Simpler Kafka Explanation : https://www.gentlydownthe.stream/
##### Motivating Example
![[Pasted image 20260302230251.png]]
##### Overview
1. Broker 
	1. Servers that holds queue 
2. Partition 
	1. The Queue
	2. Ordered immutable sequence of messages that we append to like a log file. 
	3. Each broker can have multiple partitions 
3. Topics
	1. Logical Grouping of Partitions. 
	2. Topics is about organizing the data while partition is about scaling the data.
	3. We publish to and consumes from the topics in Kafka. 
4. Producers & Consumers 

##### Lifecycle of Message 
1. Producer create & publish a message/records
	1. Headers
	2. Key
	3. Value
	4. TimeStamp 
2. Kafka assigns the message to the correct topic, broker and partition ![[Pasted image 20260302234138.png]]
3. Message is appended to partition via append only log file ![[Pasted image 20260302234623.png]]
4. Consumers then reads the message based on offset ![[Pasted image 20260302234741.png]]
5.![[Pasted image 20260302234917.png]] 
5. Pseudo Code 
	1. Initialize the Kafka Client
	2. Initialize the producer
	3. connecting the producer
	4. Sending message to the topic with keys
```java
@Autowired private KafkaTemplate kafkaTemplate;

// Step 1: Initialize the Kafka Client (via ProducerFactory configuration)
@Bean 
public ProducerFactory producerFactory() {
 Map configProps = new HashMap<>();
 configProps.put("bootstrap.servers", "localhost:9092");     
 configProps.put("key.serializer",  "org.apache.kafka.common.serialization.StringSerializer"); configProps.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer"); 
 return new DefaultKafkaProducerFactory<>(configProps);
 
 
 // Step 2: Initialize the Producer (as KafkaTemplate) 
@Bean 
public KafkaTemplate kafkaTemplate(ProducerFactory producerFactory) { 
	return new KafkaTemplate<>(producerFactory); 
}

@Override public void run(String... args) throws Exception { 
String topic = "my-topic";
for (int i = 0; i < 5; i++) { 
	String key = "key-" + i; 
	String message = "Message with key: " + key; 
	ListenableFuture> future = kafkaTemplate.send(topic, key, message);
	
	future.addCallback(new ListenableFutureCallback>(){ 
		@Override 
		public void onSuccess(SendResult result){ 
			System.out.println("Sent message=[" + message + "] with offset=[" + result.getRecordMetadata().offset() + "]"); }

		@Override 
		public void onFailure(Throwable ex) { 
		System.err.println("Unable to send message=[" + message + "] due to: " + ex.getMessage()); } });

```
3. 
##### When to use Kafka 
- As a message queue:
    - Asynchronous workloads (e.g., YouTube video upload → enqueue “transcode” job; worker processes later).
    - Rate smoothing / waiting rooms (e.g., Ticketmaster wait queue, LeetCode judge submissions) so producers and workers can scale independently.​
- As a stream:
    - Real‑time analytics (e.g., ad click aggregation), metrics pipelines, or pub/sub fan‑out (e.g., live comments fan‑out to many viewers).​


##### What we should know about Kafka for deep dive interviews
1. Scalability
	1. Constraints - aim for <1 MB per message and One broker up to 1 TB data & 10k messages per seconds. 
	2. More brokers 
	3. Choose a good partition key - evenly distribution 
	4. How to handle a hot partition ?
		1. Remove the key, thus random even distribution - only when we don't care about ordering of the messages. 
		2. Compound Key: team::OrgId 
		3. Backpressure - slow down the producer
2. Fault Tolerance and Durability 
	1. Relevant Settings 
		1. Acks (acks = all, that is, all followers need to ack that they received the message)
		2. replication factor, by default, it is 3. 
	2. What happens when consumers go down?
		1. we can read the latest offset, ![[Pasted image 20260303000048.png]]
		2. If a consumer group, rebalance
3. Errors and Retries 
	1. Producer Retries 
		1. Graceful retries 
			1. retries : 5
			2. intitialRetryTime : 100
			3. idempotent: true 
	2. Consumer Retries 
		1. ![[Pasted image 20260303000237.png]]
4. Performance Optimization 
	1. Batch messages in the producers 
	2. Compress Messages in producers 
5. Retention Policies 
	1. retention.ms - how long we should keep the message - 7 days by default.
	2. retention.bytes - when to start purging based on size - 1 GB by default


![[c1f19f6d-cd49-474b-859f-629cfc7df7e7.png]]![[7a6b4d63-d856-4e07-8cf4-f0733872fc0a.png]]![[177a5621-c556-45f3-8675-89d2c3e3d9ef.png]]![[1be77643-7ba0-4d95-b37f-b4d3e52ed1af.png]]**

# **![[2f752765-f0d8-45c6-91b4-1d68136f09f5.png]]**

##### What is Apache Kafka?
1. Durable messaging system that exchanges data b/w processes, applications and servers 
2. Publish-Subscribe based durable messaging system. ![[Pasted image 20260302192822.png]]


- Application connect to the system and transfer record onto the topic. 
-  Topics can be defined as category. 
- Another application connects to system and process/re-process the records from a topic
- Data sent is stored for specified retention periods 

What are Records
1. Are byte [] that can store any object in any format 
2. Records Attribute
	1. Mandatory: key, value
	2. Optional: timestamp, headers 

Kafka Architecture
1. Producers 
	1. sends records to a broker 
	2. pushes records into Kafka Topics within the broker 
2. Broker
	1. handle all requests(produce, consume, metadata) from clients
	2. Keeps data replicated within the cluster 
	3. Note: There can be more than one broker within the cluster. 
3. Zookeeper :
	1. Maintains state of the cluster.
4. Consumer 
	1. Consumes batches of records from the broker. 
	2. Pulls records off a Kafka topic

Subscribe to records from topics 

##### Kafka Broker
1. Kafka cluster consists of one or more servers/brokers 
2.  It is better to have more than one broker to get the benefits of [[Data Replication]]![[kafka-broker-beginner.png]]
3. Management of brokers in a cluster is carried out by zookeeper 

##### Kafka Topic
1. Topic is a category/feed name to which records are stored/published. 
2. All Kafka records are organized into topics 
3. Producers writes to topic and consumers read from topics

##### Offset 
1. Why Offset 
	1. Enabled at-least once processing 
2. Offset is position of the message within the partition log. 
3. Offsets are unique only within a given partition. 
4. Producers
	1. Each partition is append only log 
	2. Messages are written sequentially and Kafka assign them offset 0,1,2,3.....
5. Consumer 
	1. Tracks the last offset successfully processed 
	2. Ask Kafka for next one. 
	3. Consumer periodically commit their current offset back to Kafka. 

##### How a topic is partitioned
- When you create a topic, you configure a partition count (say 10); Kafka will create 10 append‑only logs (partitions) for that topic, spread across brokers.​
- Each record sent to that topic goes to exactly one partition; topics themselves never “hold” data, their partitions do
- Data replication is implemented at partition level. 
- Each partition has one server acting as a leader and rest of them as followers. 
- Leader Replica handles all read-write request & followers replicate the leader. 
- If leader fails, one of the follower becomes the leader. 
- When a producer publishes a record to a topic, it is published to its leader. The leader appends the record to its commit log and increments its record offset. 
- Producer must know which partition to writes to, this is not up to the broker. 
- Producer attaches a key to the records. All records with same key will arrive at the same partition. 
- Before producer sends any records, it requests metadata about the cluster from the broker. Meta data contains info about which broker is the leader, producer always writes to leader. 
- A common error when publishing records is setting the same key or null key for all records, which results in all records ending up in the same partition and you get an unbalanced topic.


##### Two Types of Consumers 
1. Low Level 
	1. topics and partitions are specified as is the offset from which to read, either fixed position, at the beginning or at the end.
2. High level
	1. Also known as consumer groups
	2. CG is created by adding the property, group.id to a consumer.
	3.  By using consumer groups, consumers can be parallelized so that multiple consumers can read from multiple partitions on a topic, allowing a very high message processing throughput.


Role of Broker in Consumption 
1. The broker will distribute according to which consumer should read from which partitions and it also keeps track of which offset the group is at for each partition. 
2. It tracks this by having all consumers committing which offset they have handled.
3. Every time a consumer is added or removed from a group the consumption is rebalanced between the group. 
4. **Records are never pushed out to consumers, the consumer will ask for messages when the consumer is ready to handle the message.**


## **Record flow in Apache Kafka**

We have an example, where we have a broker with three topics, where each topic has 8 partitions.
![[consumer-group-kafka.png]]
The producer sends a record to partition 1 in topic 1 and since the partition is empty the record ends up at offset 0.
![[apache-kafka-partition.png]]
Next record is added to partition 1 will and up at offset 1, and the next record at offset 2 and so on.
![[apache-kafka-partitions-2.png]]
This is what is referred to as a **commit log**, each record is appended to the log and there is no way to change the existing records in the log. This is also the same offset that the consumer uses to specify where to start reading.

##### Apache Kafka Examples**
1. Website Activity Tracking 
	1. One topic is named "click" and one is named "upload".
	2. Partitioning setup is based on the user's id. A user with id 0, will map to partition 0, and the user with id 1 to partition 1
	3. The "click" topic will be split up into three partitions (three users) on two different machines.
	4. A user with user-id 0 clicks on a button on the website.
	5. The web application publishes a record to partition 0 in the topic "click".
	6. The record is appended to its commit log and the message offset is incremented.
	7. The consumer can pull messages from the click-topic and show monitoring usage in real-time, or it can replay previously consumed messages by setting the offset to an earlier one.![[apache-kafka-partitions-topics.png]]
2. Web Shop![[apache-kafka-web-shop.png]]
3. Application health monitoring![[kafka-application-health-monitoring.png]]

##### Kafka as a Database
1. Apache Kafka has another interesting feature not found in RabbitMQ - log compaction.
2. Log compaction ensures that Kafka always retains the last known value for each record key.
3. Kafka simply keeps the latest version of a record and deletes the older versions with the same key.![[kafka-as-a-database.png]]

##### Kafka Message queue
1. built-in partitioning
2. [[Data Replication]],
3. fault tolerance 
4. 