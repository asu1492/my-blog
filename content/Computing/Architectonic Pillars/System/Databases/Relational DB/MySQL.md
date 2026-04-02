---
source: https://app.slack.com/client/T022QFTP49L/C02V7HRCJE6
---
What is MySQL

1. developed by Swedish coy MySQL AB, acquired by Sun Microsystem and later by Oracle. (Sakila the dolphin) 
2. popular because it was key component in LAMP stack (linux OS, Apache Web Server, MySQL Databases, PHP scripting language) 
3. dual licence 
	1. commercial 
	2. GNU GPL(General Public Licence) -- led to dev of MariaDB 
4. It is object RDBMS 

Working with MySQL 

1. supports many OS, range of languages for client application development
2. LOAD DATA statement quickly reads rows from a text file into a db table. 
3. primarily works with relational db, but also supports json. 

**MySQL Storage Engines** 

1. Supports multiple SE(storing and accessing data) 
2. SE is a component that handles the SQL operations on a table and defines what features table can use. 
3. **InnoDB** is default MySQL SE. It supports 
	1. transactions -- consistency of data (ACID Compliant) 
	2. row level locking -- improves multiple user performance  
	3. clustered indexes on primary key to increase the performance of regularly executed queries 
	4. foreign keys -- constraints to maintain data integrity. 
4. MyISAM Engine
	1. manages non transactional tables 
	2. mainly read operations, with few updates 
	3. for data warehouse and web applications 
	4. uses table level locking which inhibits performance in a read/write environment. 
5. NDB 
	1. supports multiple instances of MySQL running in a cluster 
	2. ensures high level of avl and redundancy 
6. Memory 
	1. formerly HEAP 
	2. provides in memory tables 
	3. stores all data in RAM for faster access than storing data on disks. 
7. Merge 
	1. treats groups of MyISAM tables as a single table. 
8. EXAMPLE
	1. Allows developers to practice creating a new storage engine.
	2. Allows developers to create tables.
	3. Does not store or fetch data.
9. ARCHIVE
	1. Stores a large amount of data.
	2. Does not support indexes.
10. CSV
	1. Stores data in Comma Separated Value format in a text file.
11. BLACKHOLE
	1. Accepts data to store but always returns empty.
12. FEDERATED
	1. Stores data in a remote database.

```
SHOW ENGINES; 

## Creating table using a specific storage engine 
CREATE Table tableName (i INT) ENGINE = INNODB;  || If engine is not specified, default engine in INNODB

## Changing storage engine for current session 
SET default_storage_engine = ARCHIVE; 

## Setting default storage engine for all session
set the default SE in the my.cnf 

## Convert a table from one SE to another 
ALTER TABLE tableName ENGINE = Memory; 
```

MySQL supports Scalability and Availability options 

1. Replication 
2. Two clustering options 
	1. InnoDB storage engine -- one read write primary server and multiple secondary servers. 
	2. NDB storage engine -- multiple MySQL server nodes access a set f data nodes(usually stored in memory). Running multiple data nodes provides redundancy and hence increases avl in the event f failure. Running multiple server nodes provides for scalability 

* * *

The tables in the mysql database fall into several categories, some of which include:

* Grant System Tables
* Object Information System Tables
* Log System Tables
* Server-Side Help System Tables

![[Computer Science/Databases/_resources/MySQL.resources/instructions.md.pdf]]

![[MYSQL1.pdf]]

![[MYSQL2.pdf]]

![[MYSQL3.pdf]]

* * *

MySQL is also avl in cloud through

1. Self Managed (VM Images and Containers) 
2. As Managed Services, can run on 
	1. IBM Cloud 
	2. Azure DB 
	3. Amazon RDS 
	4. Google Cloud SQL 

MySQL can be accessed through 

1. mySQL CLI 
2. mysqladmin CLI 
3. phpMyAdmin Web interface 
4. MySQLWorkbench is a desktop app 
	1. Schema page to access objects in db 

**Creating and Using DB**

1. CREATE DATABASE dbName;
2. USE dbName 
3. CREATE TABLE tableName (
4.  firstName VARCHAR(20), 
5.  lastName VARCHAR(20)
6. )

|     |     |
| --- | --- |
| Backup | mysqldump -u root name\_of\_db > name\_of\_file.sql <br><br>IN mysql CLI : <br>mysql > source name\_of\_file.sql |
| Restore | mysql -u root destination\_db < name\_of\_file.sql |
| Loading | fine for small amt of data <br>INSERT INTO Table tableName (<br>); |
| Importing | load data infile 'file\_data.csv' into table table\_name <br><br>mysqlimport table\_name file\_data.csv |
| Showing structure of newly created table | DESCRIBE table\_name; |
|     |     |
