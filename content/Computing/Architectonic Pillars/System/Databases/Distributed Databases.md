---
source: https://www.coursera.org/learn/introduction-to-nosql-databases/quiz/WpXF1/practice-quiz-working-with-distributed-data/attempt?redirectToCover=true
---
Distributed Systems
1. What
	1. DD are collection of multiple interconnected databases, spread physically across various locations and connected via a network connection. Distributed by fragmenting and replicating data. It follows BASE consistency model   
	2. DB distributed on a cluster of servers for 
		1. large scale workloads / mission critical 
		2. high avl or scalability requirement 
2. Adv 
	1. more reliability and avl 
	2. improved performance, esp fr high vol data, 
	3. reduced query time 
	4. ease f scale/growth 
	5. continuous avl 
3. Dis 
	1. concurrency control -- consistency of data 
		1. Developer driven consistency of data can be achieved via 
		2. WRITES/READS to a single node per fragment of data -> data is sync in background 
		3. WRITE operations go to all nodes holding a fragment of data, READS to a subset of nodes per consistency 
	2. No transaction support / or very limited 

**Fragmentation**
1. Input data is fragmented and stored in different DB 
2. Fragmenting also known as sharding or partitioning 
3. Very large tables are split across multiple logical partition, with each partition containing a subset of the overall data. 
4. When these partition are placed on separate nodes in a cluster, it is called sharding. **Each shard contains its compute resources** -- Processing, Memory and Storage to act on its subset/partition of data. Client query is processed in parallel on multiple nodes or shards of the db, query result from dif nodes is synthesized and returned to the client. 
5. Additional shards and nodes can be added if workload increases, or to improve performance.
6. Data Partitioning and Sharding are more common for data warehousing and BI.  
7. F is done either by 
	1. Grouping keys lexically, e.g. all records starting with A or A-C 
	2. Grouping records by key , e.g. all records with key storeID =123
8. ![[image (11).png]]



* * *

DDD Architecture (disk distributed database) includes

1. Shared Disk Architecture (SDA)
2. Shared Nothing Architecture 

SDA

1. share common storage 
2. multiple db server process the workload in parallel, ensuring faster data processing & easier scalability and high availability(failure resilient)  . 
3. ![[image (9).png]]
4. 

SNA 

1. Replication 
	
2. Partitioning 
	1. 

* * *

What is one way that a distributed NoSQL database usually shards data?

**1** **/** **1** **point**

By distributing all records that share the same key across multiple servers

By grouping all keys numerically

By grouping all records that have the same data on the same server

By grouping all keys lexically

**Correct**

All distributed systems need to expose a way to store a large chunk of data. For example, all keys that start with A can be found on a specific server.

### **2****.**

Question 2

Which requirement would prompt you to consider choosing RDBMS over NoSQL?

**1** **/** **1** **point**

Easy scalability

Flexible schema

Complicated joins

High availability

**Correct**

NoSQL systems, by design, do not support transactions and joins (except in limited cases).

### **3****.**

Question 3

What requirement does the BASE model support?

**1** **/** **1** **point**

Immediate consistency

Isolated consistency

Eventual consistency

Atomic consistency

**Correct**

A BASE data store values availability over consistency, but it doesn’t offer guaranteed consistency of replicated data at write time.

### **4****.**

Question 4

What is the best way to describe the Partition Tolerance characteristic of the CAP Theorem?

**1** **/** **1** **point**

The system continues to operate even in the presence of node failures.

The cluster must not continue to work during communication breakdowns between nodes in the system.

All nodes within a cluster see all the data they are supposed to at the same time.

The system continues to operate despite data loss or network failures.

**Correct**

Partition Tolerance means that a given system continues to operate even under circumstances of data loss or network failure. The cluster must continue to work despite any number of communication breakdowns between nodes in the system.
