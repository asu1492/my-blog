---
source: https://btholt.github.io/complete-intro-to-databases/postgresql
---
RDBMS 

1. set of software tools that controls the data access, organization and storage. 

1. Well developed Relational Databases should be
	1. Accurate
	2. Easy to access
	3. Reliable — data integrity and consistency
	4. Flexible — meet future requirement
2. E.g. are
	1. MySQL(backed by Oracle, FB uses it)
	2. PostgreSQL
	3. MariaDB(open source)
	4. Amazon Aurora
	5. SQLite(IoT devices, not recommended for )
	6. Microsoft SQL Server
	7. DB2 by IBM   
3. rows and columns 
4. very structured schema 
5. very good at describing relations, that is, having multiple tables relating to each other. 
6. SQL is structured query lang -- that is well defined grammar. 
7. Single source of truth -- should not duplicate it -- store it at once and use it everywhere 
8. One to many relationship -- e.g. one user have many comments 
9. links b/w tables are defined in a way that minimizes the duplication of data 
10. Are primarily [OLTP System](https://www.evernote.com/shard/s450/nl/82013406/d4d1079c-c93c-4cf1-ba89-c7b0f9d37b87)

* * *

* * *

1. Difference b/w Information and data model 
2. Adv of the Relational Model 
3. Diff b/w an entity and an attribute 

|     |     |
| --- | --- |
| Information Model vs Data Model |     |
| 1. IM is an abstract, formal rep of entities that includes properties, relationships and the operations. <br>2. Entities being modeled can be from the real world, such as library. <br>3. IM is at conceptual leve and defines rel b/w objects. <br>4. E.g. Hierarchical Info Model - organizes data using tree structure ; parent node followed by child node ; child node cannot have more than one parent, however parent node can have multiple children . First was IBM's Information Management System, released in 1968. It was originally built as the DB for the Apollo Space Mission. | 1. DM are concrete, specific model for implementation. <br>2. E.g. Relational Model is most used DM. Data is stored in tables. Provides for logical data independence, physical data independence and physical storage independence. <br>3. E.g. Entity Relationship DM<br>	1. Alternative to ER Data Model.<br>	2. ERD(ER Diagram) rep entities(called tables) and their relationships. ERD are the foundation for designing a database. <br>	3. Used as a tool to design relational DB.<br>	4. Entities are objects like a noun. It exist independently of any other entities in DB.<br>	5. ERD can be converted into a collection of tables. <br>	6. ERD building blocks are entities & attributes. <br>	7. Entities have attributes. Attributes are the data elements that characterize the entity.<br>	8. In ERD, entities are drawn as rectangle and attributes are drawn as oval. <br>	9. Entity becomes a table in the DB and attributes become the columns. |

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.1.jpeg]]

* * *

**ERD and Type of Relationship**

Building Blocks of Relationship 

1. Entities -- rep by rectangle 
2. Relationship Sets -- rep by diamond
3. Crows Foot Notations -- >, <, |

Types f Rel 

1. One to one
2. One to Many 
3. Many to Many Rel 

One to One 

1. One entity is associated one and only one instance of another entity. 

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/image.png]]

One to Many

1. One book written by many authors 

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/image (1).png]]

Many to Many

1. Many authors writing many books or many books written by many authors 

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/image (2).png]]

* * *

ER Diagram translation into Relational Table(Rows and Columns)  

1. Entity becomes table 
2. Attributes becomes columns 
3. Data values becomes rows 

* * *

Commonly used Data types in RDBMS 

1. Character String 
	1. Fixed Length DT -- CHAR(10); 10 rep character space
	2. Variable Length DT  -- VARCHAR(20) ; 20 rep max length for the string 
2. Numeric 
	1. Integer 
		1. INT 
		2. SMALLINT 
		3. BIGINT 
	2. Decimal (varies across databases) 
		1. DECIMAL
		2. NUMERIC
		3. FLOAT
		4. SINGLE 
		5. DOUBLE
		6. DEC
		7. DECFlOAT
	3. uses 2 or 4 bytes of storage to a
3. Date / Time 
	1. Date
	2. Time 
	3. Timestamp 
	4. ![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/image (3).png]]
4. Boolean 
5. Binary String 
	1. Rep image, voice or other media data 
6. LOB 
	1. Large object such as a file 
	2. this type of data is stored outside of the main DB 
7. XML
	1. stores platform agnostic unstructured data in a hierarchical form 
8. User defined data types (UDTs) 

Adv f using datatypes
1. Data integrity 
2. Data sorting 
3. Range selection 
4. data calculation 
5. use of std functions -- sum, avg, etc. 

* * *

* * *

Relational Model 

1. Relational Terms -- relation, degree, cardinality) 
2. Diff b/w 
	1. Relational Schema 
	2. Relational Instance 

RM, first proposed in 1970. Based on mathematical model and terms. Building blocks of RM are

1. Relation - mathematical concept based on the idea of sets; Relation is mathematical term for table; Relation is made of  
	1. Relation Schema 
	2. Relation Instance 
2. Sets - unordered collection of distinct elements, items of same type, have no order and no duplicates  

Relation Schema 

1. specifies name of a rel, name and datatype of each column(attributes) 
2. ![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/image (4).png]]

Relation Instance 

1. table made of rows/tuples and columns/attributes  
2. columns are attributes; degree refers to the no. of attributes  
3. Rows are tuples ; cardinality refers to no. of tuples 

* * *

* * *

Which of the following statements about Data models is correct?

**1 / 1 point**

A Data model defines the relationships between objects.

A Data model is and abstract, formal representation of entities.

A Data model is the blueprint of any database system.

A Data model describes information at a conceptual level.

**Correct**

A data model is the blueprint of any database system.

### **2.**

Question 2

Which two types of relationship does Crows foot notation represent ?

**1 / 1 point**

Many-to-many

**Correct**

Crows foot notation can be used to represent many-to-many relationships, such as many books being written by many authors.

One-to-many

**Correct**

Crows foot notation can be used to represent one-to-many relationships, such as one book with many authors.

Multiple primary

One-to-one

### **3.**

Question 3

Entity Relationship Diagrams (ERDs) are the foundation for designing databases. After creating an ERD, what is the first step you must take to map the ERD to the table?

**1 / 1 point**

Arrange the attributes by importance

List the attributes alphabetically

Separate the entity from the attributes

None of the above

**Correct**

Separating the entity from the attributes helps clarify the table (entity) and the columns (attribites).

### **4.**

Question 4

Which of the following is **NOT** an advantage of using data types?

**1 / 1 point**

Range selection

Data sorting

Use of standard functions like AVG(), MIN(), MAX(), and SUM().

Auto-correct

**Correct**

This is not included in the advantages of using data types.

### **5.**

Question 5

What are the building blocks of the Relational Model?

**1 / 1 point**

Mathematical model and terms

Index and Elements

Relations and sets

Collections and Items

**Correct**

Building block of Relational Models are: Relations and Sets.

* * *

* * *

**Database Architecture**

1. Deployment Topologies for databases 
2. 2 Tier and 3 Tier Architectures including their layers such as 
	1. database drivers
	2. interfaces 
	3. APIs

Deployment Topologies (4 types) 

1. Local/Desktop 
2. Client / Server 
	1. 2 tier
	2. 3 tier 
3. Cloud 

Local 

1. Resides on the user's system 
2. access often limited to single user
3. also known as single tier architecture 
4. use case 
	1. development/test
	2. database embedded in a local application  

Client / Server 

1. DB resides on a DB server 
2. users access db from client systems, often through web page of local application. 
3. Sometimes there is a middle tier (Application Server Layer), b/w application client and the remote db server. 
4. 2-Tier DB Architecture 
	1. Client Application and Db server run in separate tiers. Application connects to db through an API or framework( dependent on the programming lang the application is written in). DB interface communicates with the db server through a db client or API installed on client system.
	2. DBMS(DB Management system) software on the server includes multiple layers which on a high level can be categorized as - 
		1. Data Access Layer server includes interfaces for diff types of clients, which can include data industry std APIs such as JDBC, ODBC; CLP(command line processor) interfaces; vendor specific or proprietary interfaces  
		2. Database Engine Layer compiles queries, and retrieves and processes the data and returns the result set 
		3. Database Storage Layer or Persistent Layer is where data is stored which may on local storage or resides physically on network storage or specialized storage appliances 
		4. ![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/image (7).png]]
5. 3 Tier DB Architecture 
	1. Application Presentation Layer and Business Logic Layer resides in different tiers 
	2. User interacts with a client presentation layer such as mobile application, 
	3. Client Application communicates with application server over the network. Application server encapsulates the application and business logic and communicates with the db server through a db API or driver. 
	4. ![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/image (8).png]]
6. use case
	1. commonly used for multi user scenario and typical of production environments. 

                    ![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/image (5).png]]

Cloud 

1. DB resides in a cloud env 
2. no req of downloading or installing software and maintaining supporting infra
3. easy for users to access it from wherever they are 
4. Client access db through an application server layer or interface in the cloud 
5. Very flexible, used for development, testing and full production environments. 

            ![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/image (6).png]]

* * *

* * *

3 Main Classes of Users 

1. Data Engineers and DB Administrators 
2. Data Scientists and Business Analyst 
3. Application Developers and Programmers 

Data Engineers and DB Administrators 

1. access the database for admin tasks such as creating and managing db objects, setting access controls, monitoring and performance tuning 
2. They make use of following mechanism (DB can be accessed through)
	1. GUI or web based tools 
	2. Command Line tools and scripts, e.g. db2 create database Abhishek ; SQL Scripts and batch files that are executed from shell 
	3. APIs and ORMs 

Data Scientists and Business Analyst 

1. Analyzing data in db for deriving insights from data and making data driven predictions. 
2. Only read, sometimes also creates db objects in their own sandbox env. 
3. DS and BA tools interfaces with relational db using SQL interfaces and APIs. 
4. DS Tools includes
	1. Jupyter 
	2. R Studio
	3. Zepplin 
	4. SAS
	5. SPSS 
5. BA tool includes 
	1. Excel
	2. Cognos
	3. PowerBI
	4. Tableau 
	5. MicroStrategy 

Application Developers and Programmers 

1. make use of prog lang; These lang talks to db using SQL Interfaces and APIs such as JDBC, ODBC, Rest APIs(used by cloud db) 
2. These days most programmers make use of ORM(Object Relational Mapping) Frameworks for working with db, as they are easier to use and mask the complexity f underlying RDB and SQL. E.g. of popular ORM Framework includes 
	1. ActiveRecord in Ruby
	2. Django in Python 
	3. Entity framework in .NET
	4. Hibernate in Java 
	5. Sequelize in Javascript 

* * *

* * *

History of RDB 
![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/image (12).png]]

Open Souce DB 

1. MySQL by Oracle 
2. PostgreSQL
3. SQLite by Dwayne Richard Hipp 
4. have risen in popularity with 50% of market share 

Commercial DB 

1. Oracle DB
2. Microsoft SQL Server
3. IBM Db2 

Rise in popularity of Cloud DB 

1. driven by corporations moving to SaaS model (Software as a service) 
2. highly scalable 
3. By 2022, 75% f all db wl be cloud based (Gartner)
4. E,g,
	1. Amazon Redshift ; Amazon DynamoDB 
	2. Microsoft Azure SQL DB 
	3. Microsoft Azure Cosmos DB 
	4. Google BigQuery 

* * *

**Key, Constraints and Normalization** 

Primary Key 

1. On one column or combination of columns 
2. cannot contain Null 
3. Key definition enforces uniqueness on the column or combination of columns 
4. automatically creates indexes on the column 
5. can also be programmed for automatic generation of incremental numbers 

Foreign Key 

1. corresponds to PK of another table 
2. used for establishing relationship b.w the tables 
3. Actions
	1. ON Delete 
	2. ON UPDATE 

Normalization 

1. Most OLTP Systems are N to 3rd NF for optimal transaction performance. 
2. OLP systems are denormalized to enhance performance. 

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/Normalization, Keys and Constraints.pdf]]

* * *

* * *

1. Role Database [[Data Replication]] play in Distributed DB Architecture 
2. three main classes of database users are more likely to use Object Relational Mapping (ORM) in their workloads?
3. MySQL supports multiple storage engines. Which of the following supported storage engines use table-level locking?
4. PostgreSQL is an object-relational database management system. What does the object part mean for PostgreSQL?
5. A Relation is a table made up of columns and rows. Columns are attributes or fields. What are rows?

Role of DDA

1. High Avl 
2. Improved Performance 
3. Disaster Recovery 

* * *

* * *

Database Design 

1. Imp f DD 
2. DBD Processes 
3. Purpose of an ERD Tool 

Imp f DbD 

1. for success of a project 
2. helps in 
	1. integrity of data 
	2. reducing redundancy 
	3. performance 
	4. user satisfaction 
3. avoid costly problems 

DBD Processes

1. RA (Requirement Analysis) 
2. LD (Logical Design) 
3. PD (Physical Design ) -- how to implement LD in RDMS 

RA

1. identification of base objects and the rel b/w these objects 
2. identify info associated with these objects 
3. reviewing existing store for data -- RDB, physical record, etc. 
4. interviewing users or potential users on how they use this data or how can we make better use of data 
5. Output of RA can be a report, data diagram, etc.  Sharing of such report with stakeholders to validate your understanding of the system. 

LD

1. Mapping of entities, attributes and relationship 
2. Entities are objects and characteristics of objects become attribute 
3. Entities can be 
	1. People 
	2. Events 
	3. Locations 
	4. Things 
4. Many to many relationship can lead to ambiguity in db. Can be solved by introducing associative entity 
5. After LD, Normalization is carried out. 
6. ![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.3.png]]
7. ![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.png]]However, above will not adhere to 2NF. 
8. ![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.12.png]]

PD

1. Impact of choice of your design on DBMS 
2. e.g datatypes it support, naming rules it implement, indexes and constraints it supports 
3. ![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.6.png]]

* * *

API Calls
![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.7.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.4.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.2.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.14.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.8.png]]

The ibm\_db API provides a variety of useful Python functions for accessing and manipulating data in an IBM data server database, including functions for connecting to a database, preparing and issuing SQL statements, fetching rows from result sets, calling stored procedures, committing and rolling back transactions, handling errors and retrieving metadata.

Connecting to the DB2 requires the following information: a driver name, a database name, a host DNS name or IP address, a host port, a connection protocol, a user ID, and a user password.

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.5.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.10.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.13.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.9.png]]

Stored Procedures 
![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.11.png]]

CALL UPDATED\_SAL('E1001', 1) 

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.15.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.21.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.17.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.18.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.16.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.20.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.19.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.23.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.24.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.22.png]]

* * *

* * *

![[CREATE and DROP tables.pdf]]

![[CREATE, ALTER, TRUNCATE, DROP.pdf]]

![[Create Tables using SQL Scripts and Load Data into.pdf]]

![[Create Tables using SQL Scripts and Load Data into.1.pdf]]

![[String Patterns, Sorting and Grouping.pdf]]

* * *

* * *

Essentially, applying **GROUPING SETS** to the two dimensions, salespersonname and autoclassname, provides the same result that you would get by appending the two individual results of applying GROUP BY to each dimension separately

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.26.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/Relational_Databases.resources/unknown_filename.25.png]]
