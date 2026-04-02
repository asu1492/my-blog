---
---
Small Data vs Big Data
![[image (27).png]]

* * *

**Big Data : High**  

* Volume
* Velocity 
* Variety 
* Veracity - certainty f data 
* Fifth Value : better decisions and new market opportunity 
* ![[image (32).png]]![[image (31).png]]![[image (30).png]]

![[Computing/Architectonic Pillars/System/Data Engineering/_resources/Data.resources/unknown_filename.2.png]]

BD Use Case

* ![[image (33).png]]

* * *

![[image (28).png]]

* * *

BD is the digital trace that gets generated through the entire digital ecosystem. 

What is Data?

* It is an unorganized info, processed to make it meaningful. 
* consist up of 
	* facts, observations, perceptions
	* number, characters and symbols
	* images and mix of these. 

Based on Level and Rigidity, Data can be categorized into (Data collection in following formats)  

1. Structured 
	1. rigid structure
	2. well defined schema 
	3. organised into rows and columns (tabular schema)
	4. e.g. RDBMSes and Excel Sheets
2. Semi Structured 
	1. organized in hierarchy using tags and metadata
	2. e.g. XML, JSON 
3. Unstructured
	1. does not follow any existing format, sequences, semantics or rules. 
	2. often stored in NoSQL databases 

* * *

Data Sources

1. RDBS
2. Flat files and XML Datasets 
3. APIs and Web Services
4. Web Scraping 
5. Data Streams and Feeds 
6. Social Platforms 
7. Sensor Devices 

3 Types f Data Source

1. Social data'
2. machine data
3. transactional data 

![[Computing/Architectonic Pillars/System/Data Engineering/_resources/Data.resources/Image.png]]

* * *

File Formats

1. DTL 
2. Spreadsheets
3. Language Files

Delimited Text Files

* is method of representing a table of data in a text file -- indicating a structure of columns and rows.
* stores data in rows, item separated by a delimiter 
* largely used for import/export function
* common type of DTF
	* CSV : Comma Separated Values -- end of row denoted either by :; or  /n 
	* TSV : Tab seperated value -- use of tab characters to separate fields and new line to indicate new row -- used in regions where , used as decimal 
	* Others -- use of pipe character('|'), as a common choice to delimit fields 

Spreadsheets

1. XLSX 
2. Stores Data in rows and columns 

Language Files

1. XML (Extensible Markup Language)
	1. readable by both human and m/c
	2. Platform and programming language independent
	3. transfer data  
2. JSON(Javascript Object Notation)
	1. programming language independent
	2. popular choice for sharing data of any size and type, including audio and video 
	3. returned by many APIs and Web Services 

Data can be transferred in 

1. CSV
2. XML 
3. JSON 

* * *

Data Repositories 

1. Relational and Non Relational 
2. OLTP and OLAP 

OLTP 

1. Transactional or Online Transaction Processing System 
2. storing high volume of data to data operational data 
3. typically relational, but can be non rel as well 

OLAP 

1. Analytical or Online Processing System 
2. optimized for conducting complex data analytics
3. includes relational, non relational DB, data warehouses, data lakes and big data stores

* * *

Parallel Processing 
1. Why BD need PP 
2. Linear vs Parallel Processing 
3. PP and Scalability 
4. Horizontal Scalability 
5. Embarrassingly parallel 
6. Fault tolerance in parallel computing 

Linear Processing 

1. Instructions are executed one after the another. Any error results in whole instructions getting executed again. 

Parallel Processing 

1. Instructions are executed simultaneously on different nodes. Errors can be fixed locally. 
2. Adv
	1. execute large datasets in a fraction of time 
	2. requires less memory and compute requirement 
	3. execution nodes are added/removed as and when required -- reducing infra cost 

Data Scaling 

1.  Scaling Up 
2. Horizontal Scaling : Scale out -- adding horizontal nodes --
	1. Embarrassingly parallel. Failure f one node do not affect whole processing, and can simply be rerun. 
	2. Not so easy parallel - cluster is made to behave as a single computer 
3. Hadoop clusters - bringing compute to the data, computation takes place right at the location f the data. 

Fault Tolerance 

1. Ability f a system to continue operating even when one or more of its components fails. 
2. Works for HDFS and other similar storage systems like S3, Object storage 
3. maintained through [[Data Replication]] and partitioning 

* * *

**BD Tools have six main tooling categories :** 
![[image (34).png]]

![[image (39).png]]![[image (38).png]]![[image (37).png]]![[image (36).png]]![[image (35).png]]

* * *

**Open Source and Big Data**

1. OS Licence Type
2. OS Platform 

OS Licence Type

1. Public Domain 
2. Permissive
3. CopyLeft
4. Lesser General Public Licence 

OS Platforms 

1. Apache
2. Linux Foundation 
3. Cloud Native Computing Foundation 
4. Hadoop 
	1. Mapreduce -- recently replaced by Spark 
	2. HDFS -- alternate includes S3 and Object storage
	3. YARN - alternate is Kubernetes, container based resource manager. 
5. Hive and Spark -- supports ETL 
6. Apache HBase -- large noSQL datastore, manages storage and computation resourecs outside of hadoop ecosystem  
7. HDP - Hortonworks Data Platform provides BG tools that are configured to work together 

* * *

* * *

![[Computing/Architectonic Pillars/System/Data Engineering/_resources/Data.resources/unknown_filename.1.png]]

![[Computing/Architectonic Pillars/System/Data Engineering/_resources/Data.resources/unknown_filename.png]]
