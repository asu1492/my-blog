---
source: https://www.coursera.org/learn/relational-database-administration/home/week/2
---
Stages of DB Life Cycle 

* Requirements Analysis
	* is carried out by DBA along with Data Engineer
	* They establish what data is involved, talks to users and producers and sample how users wl user the data, such as reports or dashboards  
	* Work with Stakeholders includes DE, Developers, Administrators, end users, tech managers, other DBAs to 
		* analyse the need for db
		* clarify goals fr db 
		* identify users f the db  
* Design and Plan 
	* develop a plan for implementing db
	* works n logical and physical design 
	* works with db objects such as 
		* instances
		* dbs
		* tables 
		* indexes
	* DB model rep design of a db 
		* Instances containing dbs and tables, how tables relate to each other, how users access the data and so on. 
		* DBAs and db architectures model the db and their object with the help f ERDs. 
		* In case of on premise db, DBAs also need to consider size, capacity and growth. 
		* They Determine appropriate sever resources like 
			1. storage space
			2. RAM
			3. Processing power 
			4. log file size 
		* Plan for how DB objects are physically stored -- DBAs can choose to store frequently used data on a high speed disk array or store indexes separately from data for better performance. 
* Implementation  
	* DBAs roll out the carefully planned design. 
	* creates and configure db objects like 
		* instances 
		* dbs 
		* tables 
		* views 
		* indexes 
	* grant access for db users, groups 
	* also automates repeating db tasks such as backups, restores, deployments to improve efficiency 
	* Moving a project from one env to another, e.g. from Application Development Env to Production Env  
* Monitoring and Maintain 
	* monitor sys for long running queries and help end users optimize them to run faster and not overuse system resources. 
	* Review Reports to identify expensive queries, resource waits, etc 
	* Apply upgrades and patches to RDBMSes 
	* automate deployments and routine tasks for efficiency 
	* troubleshoot operational issues 
	* security and compliance --
		* ensure data is secured and only authorized users have access to it. 
		* review logs, monitor failed logins and data access activity 
		* maintains db permissions - grant and revoke access 

* * *

* * *

Fundamental Data Ethics

1. Transparency - disclose to users about storing, processing of data, disposing data once finished using it 
2. Consent before collecting data from data owner 
3. Integrity - clear about enforcement and ensure compliance across coy 

Secure system design 

1. Protect from malicious access through secured firewall and educate users about phishing and other ways
2. Secure storage from natural disaster, hardware failures -- preparedness plan and regular backup f data 
3. Accurate access 
4. secure movement f data in and out of db 
5. secure archiving 

Database objects 

1. Hierarchy of db objects 
	1. Storing tables, constraints, indexes and other db objects in hierarchical structure allows DBAs to manage security, maintenance and accessibility. 
	2. Instances : Single way f organizing the db. Many RDBMSes permit more than one db with a single instance 
	3. Databases : one or more under an instance 
	4. Schema : logical grouping f objects within a database. Schemas define how db objects are named & prevent ambiguous references. For some RDBMSes schema is a parent object f a db 
	5. db Objects : Within a schema are db objects which includes tables, constraints and indexes 
	6. ![[image (16).png]]
2. Instance of a db 
	1. logical boundary for db or set of dbs 
	2. every db within instance is assigned a unique name, has its own set of system catalog tables(which keep track f the objects within the db)  and has its own configuration files.
	3. more than one instance on the same physical server can be created. 
	4. instance allows isolation b/w dbs. 
	5. in cloud based RDBMSes, term instance means a specific running copy of a service.  
	6. ![[image (17).png]]
3. Schema
	1. schema is a specialized db object that provides a way to group other db objects logically
	2. Schema contains tables, indexes, constraints and other objects. 
	3. most RDBMses , default schema is the user schema for the current logged on user. 
	4. Many RDBMSes uses a specialized schema to hold configuration information and metadata about a particular db. e.g. tables in system schema can store lists f db users and their access permission, info about indexes on tables, details f any db partitions & user defined data types. 
	5. ![[image (18).png]]
4. DB objects 
	1. Are items that exists within the db. 
	2. process of db design includes defining db objects and their rel with each other. 
	3. create and manage db objects with graphical db mgmt tools, scripting and accessing data through an API. 
	4. If you use SQL to create or manage the object, you wl use DDL(Data Definition Langauge) statements like CREATE or ALTER. 
	5. Commonly used DBO
		1. Tables - logical structures consisting f rows and columns which store data 
		2. Constraints - data is often subject to certain restrictions or rules . e.g. employee no. must be unique 
		3. Indexes - an index is a set f pointers used to improve performances & ensures uniqueness of the data 
		4. Keys - uniquely identifies a row in a table. Key enables DBAs to define the rel b/w tables 
		5. Views - provides a diff way f rep the data in one or more tables. It is not an actual table and requires no permanent storage. 
		6. Aliases - is an alternative name for an object such as a table.  DBAs use aliases to provide shorter, simpler names to reference objects. 
		7. Events - is a DML(data manipulation language) or DDL(Data Definition Lang) action on a db object that can initiate a trigger. 
		8. Triggers - defines a set of actions performed in response to an insert, update, delete on a specified table. 
		9. Log Files - store info about transactions in a db. 
		10. ![[image (20).png]]
		11. ![[image (26).png]]

System Objects  

1. SO
	1. Purpose of SO 
	2. diff types of SO 
2. RDBMs store info about db metadata in special objects known as system db, system schema, catalog or dictionary 
3. Stores specific types f info about the db, such as name f a db or table, the data type f a column, or access privileges, known as metadata. 
4. Other term used for this info store are data dictionary and system catalog. 
5. Diff RDBMS uses diff names for their metadata stores 
	1. DB2 uses catalog(consists f tables f data about everything defined to the Db2 system) and directory(contains internal control data). Directory contains db descriptors, access path for applications, and recovery and utility status info.  
	2.  MySQL uses a system schema to store db metadata. Each new MySQL instances has 4 system db - Information\_schema, MySQL, Performance\_schema, SYS. Each sys db contains several read only tables. 
	3. PostgreSQL uses sys catalog, a schema with tables and view that contain metadata about all the other objects inside the db. It contains operations about how tables or indexes are accessed, whether the db sys is reading info from memory or fetching from disk. 

DB configuration

1. Configuration Files 
	1. How to use CF
	2. Explain common configuration settings 
	3. configuring on premises and cloud dbs
2. DB installation 
	1. supply parameters like the location f the data directory , port no that the service listens on for connections, memory allocation, connection timeout, max packet size 
	2. Can accept default options or supply custom values to suit the env. 
	3. DBI process saves this info into files known as configuration files(.conf, .cfg) or initialization files(.ini) 
3. Configuration file e.g. 
	1. Db2 uses SQLDBCONF file
	2. MySQL uses
		1. my.ini files on windows based sys
		2. my.cnf files on Linux based sys 
	3. PostgreSQL uses Postgresql.conf file 
4. Configure DB
	1. ![[image (21).png]]

* * *

* * *

1. Physical vs Logical storage 
2. Tablespaces and their benefits 
3. Storage Groups & their function 
4. When to use partition 

**Physical Storage v Logical Storage** 

1. DB storage is managed through logical db objects and physical disk files. 
2. RDBMSes separates the physical storage of db files on disk from the logical design f the db, giving greater flexibility in managing the data files. Data can be manged through a logical object w/o being concerned about nature f the physical storage. 

**Tablespaces** 

* Table-spaces are structures that contain db objects such as tables, indexes, large objects, and long data. 
* TS is used to logically organize db objects. TS defines mapping b/w logical db objects and physical storage containers that house the data for these objects. 
* TS contains can contain one or more db objects. 
* TS keeps logical db storage separate from physical storage. 
* TS benefits
	* optimized performance - can place heavily used index/file on a fast SSD. ; Rarely accessed or archived data can be stored in magnetic hard drive
	* Easier Recoverability - through a single command, can make a backup or restore all the db objects w/o worrying about which storage container each object or tablespace is stored on. 
	* Storage Mgmt - RDBMS creates and extends the datafiles or containers depending on the need. 
	* ![[image (23).png]] 

**Containers** 

* A storage container can be a data file, a directory, or a raw device 

![[image (22).png]]

**Storage Groups**

1. provided by some RDBMSes 
2. SG is largely use to organize data and storage based on temperature. 
3. SG is a grouping f storage paths or containers based on similar performance characteristics. 
4. Enables Multi Temperature Data Mgmt more easily. Temp refers to frequency of data access. 
5. Hot SG is accessed more frequently, Warm SG less frequently and Cold SG infrequently. 
6. SG optimizes performances and cost f storing data. 
7. ![[image (25).png]]

**Database Partition** 

1. DP is RDB whose data is managed across multiple partitions. 
2. Can partition tables that need to contain very large quantities f data into multiple logical partitions, with each partition containing a subset f the overall data.  
3. It is common in data warehousing and data analysis for BI 

Server Objects

Configurations

Creating and Keeping Baselines 

Performance Metrics 

1. Stress test scenario to find out the max load the database can handle. 
2. 

Monitoring RAM and Disk Usage 

Connections and Cache Stats 

Database Optimization 

1. Updating Statistics 
2. Addressing Slow Queries through query optimization 
3. Creating Indexes 
4. High read performance for meeting BI workload 

* * *

Managing Databases

1. Backup and Restore Databases
2. Security and User Management

Types f Backups

1. Point in time 
2. Full 
3. Differential 
4. Incremental 

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.14.png]]
![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.18.png]]
![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.19.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.20.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.2.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.11.png]]
![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.7.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.6.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.21.png]]
![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.16.png]]
![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.4.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.1.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.23.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.9.png]]

Backup Policy 

1. determine by recovery needs and data usage 
2. ![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.24.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.12.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.5.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.10.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.13.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.3.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.15.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.8.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.22.png]]
![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.17.png]]

Database transaction logs 

* keep track of all activities that change the database structure and record transactions that insert, update, or delete data in the database.

* * *

* * *

* Authentication is the process of verifying that the user is who they claim to be. Authorization is the process of giving users permissions or privileges to access the objects and data in the database.
* When securing a database, you need to consider the security of the server and operating system, as well as the database and data.
* A database user is a user account that is allowed to access specified database objects. Groups are logical groupings of users to simplify user management. A database role defines a set of permissions needed to undertake a specific role in the database.
* When implementing role or group membership, use the principle of least privilege.
* Auditing does not directly protect your database but does identify gaps in your security.
* Customer-managed keys provide the data owner with more control over their data stored in the cloud.

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.33.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.32.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.25.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.28.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.35.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.29.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.27.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.31.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.26.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.34.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.30.png]]
![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.53.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.37.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.47.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.50.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.46.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.36.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.41.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.56.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.51.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.38.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.40.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.42.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.49.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.44.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.54.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.48.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.57.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.43.png]] ![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.52.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.55.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.45.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.39.png]]

* * *

* * *

* Database performance is measured by using key performance indicators, known as metrics, that enable DBAs to optimize organizations’ databases.
* You should monitor at the infrastructure, platform, query, and user levels.
* A database diagnostic log file, also known as an error log or troubleshooting log, contains timestamped messages for various types of informational messages, events, warnings, and error details.
* Database optimization commands include OPTIMIZE TABLE in MySQL, VACUUM, and REINDEX in PostgreSQL, and RUNSTATS and REORG in Db2.
* Query execution plans show details of the steps used to access data when running query statements.

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.86.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.88.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.87.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.59.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.75.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.76.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.66.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.90.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.83.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.64.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.82.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.70.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.74.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.58.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.63.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.84.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.60.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.72.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.69.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.80.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.91.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.85.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.81.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.61.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.62.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.67.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.71.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.65.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.89.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.73.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.68.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.77.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.79.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.78.png]]

* * *

* * *

Troubleshooting and Automation 

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.94.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.144.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.136.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.99.png]]

Inadequate hardware in terms of 

1. Memory
2. Disk Space 
3. Processing power 

Inadequate hardware can be dealt through scaling 

1. Vertical 
	1. Increasing above three resources
2. Horizontal 
	1. Through partitioning and shards 

Proper Configuration may requires

1. More Connections 
2. Changes in buffering and indexing settings. 

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.96.png]]

Database configuration -- caching and indexing 

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.125.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.130.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.111.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.93.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.92.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.137.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.138.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.95.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.124.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.139.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.140.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.101.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.142.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.119.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.143.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.117.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.110.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.103.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.104.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.98.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.118.png]]

![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.132.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.145.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.131.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.115.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.113.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.141.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.129.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.107.png]]
![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.109.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.116.png]]

* * *

* * *

Automation 
![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.105.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.147.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.108.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.126.png]]
![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.128.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.127.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.97.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.146.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.134.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.100.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.121.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.120.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.122.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.114.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.106.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.123.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.135.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.133.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.112.png]]![[Computer Science/Databases/_resources/RDB_Administrator.resources/unknown_filename.102.png]]

* Performance monitoring, dashboards and reports, and server/database logs help identify bottlenecks.
* Database automation is the use of unattended processes and self-updating procedures for basic database tasks.
* Some automation processes include backing up, truncating, and restoring databases.
* Reports give a regular overview of database health, notifications give a forewarning of a situation that could become troublesome if not addressed, and alerts bring awareness to an issue that needs immediate attention.
