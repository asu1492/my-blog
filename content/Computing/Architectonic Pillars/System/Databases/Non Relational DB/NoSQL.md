---
source: https://www.coursera.org/learn/introduction-to-nosql-databases/quiz/SuQ23/practice-quiz-basics-of-nosql/attempt?redirectToCover=true
---
1. NoSQL is a Dumb marketing tool
2. It also known as not only SQL. It is class of databases that are non relational in architecture. 
3. Its a non relational database - document based databases, wide column, etc... 
4. Implementation of NoSQL DB differs technically, but share common traits. 
5. Schema less -- but with great power comes great responsibility -- miss spell term may end up creating a new field. 
6. Provides new way of storing and processing data. Thus can handle a different breed of scale - 'Big Data. Thus NoSQL DB have become more prevalent since 2000 due to scale demands of Big Data. 
7. Enable more specialized use cases and simpler to develop app functionality. 

**NoSQL**

* not only SQl
*  they are non relational databases(no std row and column)that refers to family of databases that vary widely in style and tech. 
* provides new way of storing and quering data 
* designed to handle data at scale -- bidgdata  

History 

1. 2000 - 2005 : Scalability of RDBMS databases were Qned. No. of companies published papers, e.g. Mapreduce by Sanjay Ghemawat and Jeff Dean.  (Era of DotCom boom, need was scalability, as tech opened from B2B to B2C) ; Amazon Dynamo paper. 
2. 2005-10 : new open source DB emerged on scene. E.g. Cassandra, CouchDB, Hbase, mongoDB,  neo4j,  riak, redis,
3. 2010 : DBaas -- Database as service -- adoption f cloud  e.g IBM Cloundant, Amazon DynamoDB

**Characteristic of NoSQL DB** 
1. Have roots in Open source community 
2. Differs technically but share certain traits, includes 
	1. scale horizontally ([[Scaling]])
	2. share data more easily than RDBMS -- use a global unique key to simplify data sharding or partitioning 
	3. they are more use case specific than RDBMS 
	4. more developer friendly
	5. flexible schema 
3. Benefits -- General
	1. Scalability -- ability to scale across server, server cluster, server racks and data centers 
	2. Performance -- fast response time even with large datasets and high concurrency must for modern applications 
	3. Availability -- more resilient as copy of data over clusters 
	4. Cloud Architecture -- clusters of servers in CA, massively reduce costs and provide scalability 
	5. Costs 
	6. Flexible schema 
	7. Varied Data Structure -- Key-value; Graph; document type
	8. Specialized capabilities 
		1. Indexing and Querying -- geospatial 
		2. Modern HTTP APIs 
		3. Data [[Data Replication]] and robustness 
4. 4 types of Non Relational DB are 
	1. ![[Computer Science/Databases/_resources/NoSQL.resources/unknown_filename.png]]

|                   | Document                                                                                                                                                                                                                                                                                                                                                                               | Column                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Graph                                                                                                                                                                           | Key Value                                                                                                                                                                                                                                                                                                                                |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Architecture      | Values are visible and can be queried; <br>each piece of data is considered a doc, e.g. JSON, XML;<br>each doc offers a flexible schema -- no two doc need to contain the same info; <br>content of doc can be indexed and queried -- key & value range lookups and search, analytical queries with mapreduce; <br><br>horizontally scalable and allow sharding across multiple nodes; | spawned from Google 'Bigtable' architecture; <br><br>also k as Bigtable clones or Columnar or Wide Colum DB; <br><br>store data in column or group of column ; <br><br>column 'families' are several rows wth unique keys, belonging to one or more column -- grouped in families as are often accessed together; <br><br>Rows in a column family are not required to share the same columns -- can share all, a subset, or none, can be added to any no of rows, or not | stores info in entities or nodes, and relationships or edges ; <br><br>impressive when your data set resembles a graph like data structure; <br><br> ACID transaction compliant | Least complex;<br>data is stored with a key and corresponding value blob & is rep by a hashmap; <br>ideal for basic CRUD Operations;<br>Scale well;<br>shard/partition easily across no. of nodes each with key-value pair;                                                                                                              |
| Architecture cons | only guarantee atomic operations on single doc;                                                                                                                                                                                                                                                                                                                                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | do not scale horizontally ; <br><br>do not shard well -- traversing a graph with nodes split across multiple servers can become difficult and hurt performance                  | not intended for complex queries ; <br>atomic for single key operations only;<br>value blobs are opaque to DB -- thus less flexible data indexing and querying                                                                                                                                                                           |
| Use Cases         | Event logging for apps and processes -- each event rep by a new doc;<br><br>online blogs -- each user, post, comment, like, or action is rep by a doc;<br><br>operational datasets and metadata for web and mobile apps -- designed with internet in mind(JSON, RESTful APIs, unstructured data)                                                                                       | great for large amt of sparse data; <br><br>can handle being deployed across clusters of nodes;<br><br>column DB can be used for event logging and blogs; <br><br>counters are unique use cases for Column DB; <br><br>columns can have TTL parameter, making them useful for data with an expiration value                                                                                                                                                              | highly connected and related data ; <br><br>social networking; <br><br>routing, spatial and map apps; <br><br>recommendation engines                                            | quick CRUD operations on non-interconnected data, e.g. storing & retrieving session info fr web applications; <br>storing in-app user profiles and preferences;<br>shopping cart data for online stores                                                                                                                                  |
| Not suitable for  | when you require ACID transaction -- cannot handle transactions that operate over multiple docs -- RDB are better suited<br><br>If data is in an aggregate oriented design -- RDB is better suited                                                                                                                                                                                     | avoid in case traditional ACID transaction -- reads and writes are only atomic at the row leve; <br><br>In early dev, query patterns may change and require numerous changes to column based DB -- costly & slows down production                                                                                                                                                                                                                                        | when application needs to scale horizontally;<br><br>when trying to update all or a subset of nodes with a given parameter -- such operations are difficult and non trivial     | interconnected, many to many relationship , e.g. social network, recommendation engines;<br><br>when we require high level of consistency for multi-operation transaction with multiple keys -- need a db that provide ACID transaction ;<br><br>when apps runs queries based on value vs key -- can use 'document' category f NoSQL db. |

* * *

**Graph Based NoSQL**
        ![[Image from iOS (1).jpg]]

**Document Based NoSQL**
        ![[Image from iOS (2).jpg]]

**Key Value NoSQL**
    ![[Image from iOS.jpg]]

**Column Based NoSQL**
        ![[Image from iOS (4).jpg]]

CRUD -- Create, Read, Update, Delete 

* * *

* * *

CAP Theorem 

1. What
2. History 
3. Relevance 

What is CAPT

1. Also known as Brewer's theorem  as first advanced Eric A Brewer. Later evolved by Seth Gilbert and Nancy Lynch  
2. Successful design, implementation and deployment of distributed system requires 3 things -> Consistency, Availability and Partition tolerance 
3. Any Distributed sys can only guarantees two of desired three characteristics 
4. can be used to classify NoSQL DB 

Partition Tolerance

1. Partition is a communication break within a system -- a lost or temporarily delayed connection b/w nodes 
2. PT means that cluster must work despite network issues. 
3. DS cannot avoid partition, thus PT, become most basic feature of DS. 
4. PT -feature of  NoSQL DB.  
5. PT is made possible by sufficiently replicating records across combinations of nodes and networks. 
6. No SQL DB can either be consistent or avl first; other -- tuneable, that is, to an extent 
	1. CP -- Consistent and PT 
	2. AP -- Avl and PT 

Availability vs Consistency 

1. Hadoop, first open Big data architecture that enabled storage and processing of large amounts of data 
2. 2000s, emergence of services that required always availability, active and accessible worldwide 
3. RDB relied on consistency, existed dialectic between A and C. 

* * *

* * *

**Challenges of Migrating from RDBMS to NoSQL DB**

1. Best Use cases of RDBMS and NoSQL 
2. Main diff b/w RDBMS and NoSQL 

Use cases

1. Both RDBMS and NoSQL caters to different use cases or requirement 
2. Soln can use both RDBMS and NoSQL 
3. Shift from RDBMS to NoSQL -- case by case basis, maybe search for 
	1. Flexibility and/or
	2. Performance 

Differences 

1. Data driven model to Query Driven Data Model 
	1. RDBMS : starts from the data integrity, relations b/w entities 
	2. NOSQL: starts from queries, not from data. Models based on the way the application interacts with the data 
2. Normalized to Denormalized Data
	1. NoSQL : think how data can be structured based on queries 
3. From ACID to BASE Model 
	1. avl vs consistency -- CAP Theorem: chose b/w C and A. 
	2. BASE: Availability, performance, geographical presence, high data volumes 
4. NoSQL system not designed to support transactions and joins, except in limited cases 

* * *

* * *

**Question 1**

What are the four main categories of NoSQL databases?

**1** **/** **1** **point**

Key-Value, Document, Common, Graph

Key-Value, Document, Column, Graph

Key-Value, Relational, Column, Graph

Key-Value, Distributed, Column, Graph

**Correct**

The four major categories of NoSQL databases all have unique characteristics that make them great fits for different types of applications or parts of applications.

### **2****.**

**Question 2**

What is the name of the fully managed service model that many NoSQL databases now leverage?

**1** **/** **1** **point**

Platform-as-a-service (PaaS)

Software-as-a-service (SaaS)

Database-as-a-service (DBaaS)

Network-as-a-service (NaaS)

**Correct**

Database-as-a-service (DbaaS) is the fully managed service model that many NoSQL databases now leverage.

### **3****.**

**Question 3**

What is the most common trait among all NoSQL databases?

**1** **/** **1** **point**

They are all open source

They are all non-relational

They are all document-based

They are all relational

**Correct**

The most common trait among all NoSQL databases is that they are non-relational in nature.

### **4****.**

**Question 4**

Which document formats are typically used for documents stored in a **document-based NoSQL database**?

**1** **/** **1** **point**

CSV and JSON format

XML and JSON format

XML and CSV format

PDF and JSON format

**Correct**

In document-based NoSQL databases, documents are typically stored in **XML or JSON format.**

### **5****.**

**Question 5**

Which category of NoSQL databases is it recommended not to shard data on?

**1** **/** **1** **point**

Document-based NoSQL databases

Key-Value NoSQL databases

Column-based NoSQL databases

Graph NoSQL databases

**Correct**

Sharding a Graph NoSQL database is not recommended since **traversing a graph with nodes split across multiple servers can become difficult and hurt performance.**

* * *

* * *

**Question 1**

What common trait does the NoSQL family of databases share?

**1** **/** **1** **point**

Technology

Tabular style

Non-relational

Exclusion of SQL

**Correct**

This is a family of databases that vary widely in style and technology, but which all share a common trait in that they are non-relational in nature (they are not a standard row and column relational database management system).

### **2****.**

**Question 2**

What may be the most common reason to use a NoSQL database?

**0** **/** **1** **point**

Availability

Scalability

Consistency

Security

**Incorrect**

Review the Characteristics of NoSQL Databases video.

### **3****.**

**Question 3**

What makes Key-Value NoSQL databases powerful for basic CRUD operations?

**1** **/** **1** **point**

They are represented as a hashmap.

The value blob is opaque.

They shard easily across nodes.

They are atomic for key operations.

**Correct**

Because Key-Value stores are represented as a hashmap, they are powerful for basic **Create-Read-Update-Delete** operations.

### **4****.**

**Question 4**

Which use case would be a poor choice for a document type NoSQL database?

**1** **/** **1** **point**

Online blogging

Event logging for a process

Operational data sets for a web app

Aggregate-oriented design

**Correct**

If the data naturally falls into a normalized tabular model, then a relational database may be a better choice.

### **5****.**

**Question 5**

What is a characteristic of column-based NoSQL databases?

**1** **/** **1** **point**

Rows in column families share a common key or identifier.

Rows in column families can share all, a subset, or none of the columns.

Columns are grouped together in families because they are often accessed individually.

Rows in column families are required to share the same columns.

**Correct**

They can share all, a subset, or none of the columns, and columns can be added to any number of rows.

* * *

* * *

Which industry will almost exclusively use ACID databases?

**1** **/** **1** **point**

Ecommerce

Online social networks

Marketing consultants

Financial institutions

**Correct**

**Financial institutions will almost exclusively use ACID databases** for money transfers because these operations depend on the atomic nature of ACID transactions.

### **2****.**

**Question 2**

What is data sharding?

**1** **/** **1** **point**

Replicating data across multiple nodes

Deleting all data and schema

Retrieving data from a failed node

Fragmenting data into smaller pieces

**Correct**

This process, which breaks data into smaller pieces for storage in a distributed system, is also called partitioning of data by some NoSQL databases.

### **3****.**

**Question 3**

How many of the desired characteristics of the CAP Theorem can a distributed system guarantee?

**1** **/** **1** **point**

None

One

Two

Three

**Correct**

A distributed system can guarantee delivery of only two of the three desired characteristics necessary for the successful design, implementation, and deployment of applications.

### **4****.**

**Question 4**

What drives your data model in NoSQL?

**0** **/** **1** **point**

The choice between availability and consistency

How the data is stored in one or more tables

Your queries and the way the app accesses the data

How the data is denormalized

**Incorrect**

Review the Challenges in Migrating from RDBMS to NoSQL Databases video.


After completing this course, you will be able to:  
\* Define the term NoSQL and the technology it references.
\* Explain the characteristics of NoSQL databases.
\* Describe the major categories of NoSQL datastores (document, key-value, graph, etc.) and their architectural differences.
\* List the most commonly used NoSQL datastores, their primary use cases and benefits (MongoDB, Cassandra, Cloudant, Couch DB, etc).
\* Understand the factors affecting return on investment for using locally hosted databases, versus hosted database versus DBaaS.
\* Describe the architecture, features, and key benefits of MongoDB as a NoSQL database.
\* Demonstrate hands-on working knowledge of MongoDB and perform various common tasks (including CRUD operations, limit and sort records, indexing, aggregation, replication, sharding)
\* Describe the architecture, features, and key benefits of Cassandra as a NoSQL database.
\* Demonstrate hands-on working knowledge of Cassandra and perform various common tasks (including using the CQL shell, keyspace operations, table operations, and CRUD operations)
\* Describe the architecture, features, and key benefits of Cloudant as a NoSQL database.
\* Demonstrate hands-on working knowledge of Cloudant and perform various common tasks (including creating the database, add documents, query data, utilize the HTTP API).

---

## Resources

- [Coursera — Introduction to NoSQL Databases](https://www.coursera.org/learn/introduction-to-nosql-databases/supplement/QDLK1/course-introduction)
- [Complete Intro to Databases (btholt)](https://btholt.github.io/complete-intro-to-databases/)