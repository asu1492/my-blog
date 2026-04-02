---
---
![[Hive Cheat Sheet.pdf]]

Interview Question 

1. Why HIVE 
2. Types of Tables avl in hive
3. Is hive suitable for OLTP System 
4. Can table be renamed in Hive 
5. Can we change datatype of a column 
6. What is metastore in Hive
7. Need for Custom Serde
8. Default location for storing table data
9. Modes in Which Hive can run 
10. datatype in hive 
11. Collection DT in Hive 
12. Running unix shell command in hive 

Why HIVE 

* tool in Hadoop ecosystem which provide interface to organise and query data in a db 
* suitable for accessing and analysing data in Hadoop using SQL syntax 

Metastore 

1. RDB 
2. stores metadata of hive tables, partitions, hive dbs, etc 

Tables in Hive 

1. Managed - both data & schema under control of HIVE
2. External - ONly schema under HIVE Control 

Renaming Table 

1. ALTER TABLE tableName RENAME TO newTableName 

Changing Datatype 

1. ALTER TABLE tableName REPLACE COLUMNS 

File Formats in Hive 

1. Hive comes with built in connectors for comma and tab-separated values (CSV/TSV) text files, Apache Parquet™, Apache ORC™, and other formats. Users can extend Hive with connectors for other formats.
2. Not suitable for OLTP as it does not provide insert and update at row level 

Custom Serde 

1. inbuilt serde may not satisfy the format of the data 
2. Need to write own java code so satisfy data format requriement 

Default location for storing table data

1. hdfs://namenode\_server/user/hive/warehouse 

Modes in which HIVE can run 

1. local
2. distributed 
3. pseudodistributed 

datatype in hive

1. TIMESTAMP datatype
2. stores data in java.sql.timestamp format 

Collection Datatypes in Hive 

1. Array 
2. Map
3. Struct 

Running unix shell command in hive 

1. !mark before command 
2. e.g. !pwd will give current directory 

Hive variable 

1. created in hive env 
2. ref by hive script 
3. pass values to hive queries when query starts executing 

Executing Hive Queries through Script files 

1. using source command 
2. HIVE > source/path/to/file/file\_with\_query.hql 

.hiverc file 

1. file containing list of commands that needs to be run when the HIVE CLI starts. 
2. e.g. setting the strict mode to be true, etc. 

functions in Hive

1. UDF
2. UDAF 
3. UDTF 

* * *

* * *

**Hive**
– Also refer to Hive User Guide <https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL>
<https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DML>
<https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select>
<https://cwiki.apache.org/confluence/display/Hive/Tutorial>
The Apache Hive™ data warehouse software facilitates reading, writing, and managing large datasets residing in distributed storage and queried using SQL syntax.
**Hive Features**

* Tools to ==enable easy access to data via SQL, thus enabling data warehousing tasks such as extract/transform/load (ETL), reporting, and data analysis==.
* A mechanism to ==impose structure on a variety of data formats==
* ==Access to files stored either directly in Apache HDFS™ or in other data storage systems such as Apache HBase™==
* ==Query execution via Apache Tez™, Apache Spark™, or MapReduce==

Hive provides standard SQL functionality, Hive's SQL can also be extended with user code via user defined functions (UDFs), user defined aggregates (UDAFs), and user defined table functions (UDTFs). Hive comes with built in connectors for comma and tab-separated values (CSV/TSV) text files, Apache Parquet™, Apache ORC™, and other formats. Users can extend Hive with connectors for other formats.
Hive is not designed for online transaction processing (OLTP) workloads. It is best used for traditional data warehousing tasks.
Components of Hive include HCatalog and WebHCat.
**HCatalog** ==is a table and storage management layer== for Hadoop that enables users with different data processing tools — ==including Pig and MapReduce — to more easily read and write data on the grid.==
**WebHCat** provides a service that you can use to run Hadoop MapReduce (or YARN), Pig, Hive jobs. You can ==also perform Hive metadata operations using an HTTP (REST style) interface==.
**Commands**
Commands are non-SQL statements such as setting a property or adding a resource

|     |     |
| --- | --- |
| Command | Description |
| quit<br>exit | Use quit or exit to leave the interactive shell. |
| Reset | Resets the configuration to the default .==Any configuration parameters that were set using the set command or -hiveconf parameter in hive commandline will get reset to default value.==<br>Note that this ==**does not apply to configuration parameters that were set in set command using the "hiveconf:" prefix for the key name (for historic reasons**==). |
| set <key>=<value> | Sets the value of a particular configuration variable (key).<br>**Note:** If you misspell the variable name, the CLI will not show an error. |
| Set | Prints a list of configuration variables that are overridden by the user or Hive. |
| set –v | Prints all Hadoop and Hive configuration variables. |
| ! <command> | Executes a shell command from the Hive shell. |
| ==dfs <dfs command>== | Executes a dfs command from the Hive shell. |

**Commands Usage:**
hive> set mapred.reduce.tasks=32;
  hive> set;
  hive> select a.\* from tab1;
  hive> !ls;
  hive> dfs -ls;
**Hive Data Types:**
Please refer complete list <https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Types>
Integers

* TINYINT—1 byte integer
* SMALLINT—2 byte integer
* INT—4 byte integer
* BIGINT—8 byte integer

Boolean type

* BOOLEAN—TRUE/FALSE

Floating point numbers

* FLOAT—single precision
* DOUBLE—Double precision

Fixed point numbers

* DECIMAL—a fixed point value of user defined scale and precision

String types

* STRING—sequence of characters in a specified character set
* VARCHAR—sequence of characters in a specified character set with a maximum length
* CHAR—sequence of characters in a specified character set with a defined length

Date and time types

* TIMESTAMP
* DATEBinary types

BINARY—a sequence of bytes
Complex Types

* Structs
* Maps (key-value tuples
* Arrays (indexable lists)

Run below Queries.
beeline
!connect jdbc:hive2://localhost:10000 cloudera cloudera

 show databases;
 use database\_name; "to use a database"
 show create tabel table\_name "view create statement of table"
 desc table\_name;
 describe table\_name;
 desc formatted table\_name;
 desc extended table\_name;
create database database\_name;
use database\_name;
drop table table \_name;
create table Aanand1.test\_1(x int);
select current\_database();
insert into table Aanand1.test\_1 values (1),(2),(3),(4);
select \* from Aanand1.test\_1 order by x desc;
alter table t1 rename to Aanand.tpc;
select min(x),max(x),avg(x),sum(x) from tpc;
**Create Table**
CREATE TABLE creates a table with the given name==. An error is thrown if a table or view with the same name already exists. You can use IF NOT EXISTS to skip the error.==
In Hive 0.13 and later, column names can contain any Unicode character (see HIVE-6013), however, dot (.) and colon (:) yield errors on querying, so they are disallowed in Hive 1.2.0 (see HIVE-10120). Any column name that is specified within backticks (\`) is treated literally. Within a backtick string, use double backticks (\`\`) to represent a backtick character. Backtick quotation also enables the use of reserved keywords for table and column identifiers.
To revert to pre-0.13.0 behavior and restrict column names to alphanumeric and underscore characters, set the configuration property hive.support.quoted.identifiers to none. In this configuration, backticked names are interpreted as regular expressions. For details, see Supporting Quoted Identifiers in Column Names.
Table and column comments are string literals (single-quoted).
A ==**table created without the EXTERNAL clause is called a managed table because Hive manages its data**==. To find out if a table is managed or external, look for tableType in the output of DESCRIBE EXTENDED table\_name.
The TBLPROPERTIES clause allows you to tag the table definition with your own metadata key/value pairs. Some predefined table properties also exist, such as last\_modified\_user and last\_modified\_time which are automatically added and managed by Hive. Other predefined table properties include:
TBLPROPERTIES ("comment"="table\_comment")
TBLPROPERTIES ("hbase.table.name"="table\_name") – see HBase Integration.
TBLPROPERTIES ("immutable"="true") or ("immutable"="false") in release 0.13.0+ (HIVE-6406) – see Inserting Data into Hive Tables from Queries.
TBLPROPERTIES ("[orc.compress"="ZLIB](http://orc.compress/)") or ("[orc.compress"="SNAPPY](http://orc.compress/)") or ("[orc.compress"="NONE](http://orc.compress/)") and other ORC properties – see ORC Files.
TBLPROPERTIES ("transactional"="true") or ("transactional"="false") in release 0.14.0+, the default is "false" – see Hive Transactions.
TBLPROPERTIES ("NO\_AUTO\_COMPACTION"="true") or ("NO\_AUTO\_COMPACTION"="false"), the default is "false" – see Hive Transactions.
TBLPROPERTIES ("compactor.mapreduce.map.memory.mb"="mapper\_memory") – see Hive Transactions.
TBLPROPERTIES ("[compactorthreshold.hive.compactor.delta.num.threshold"="threshold_num](http://compactorthreshold.hive.compactor.delta.num.threshold/)") – see Hive Transactions.
TBLPROPERTIES ("[compactorthreshold.hive.compactor.delta.pct.threshold"="threshold_pct](http://compactorthreshold.hive.compactor.delta.pct.threshold/)") – see Hive Transactions.
TBLPROPERTIES ("auto.purge"="true") or ("auto.purge"="false") in release 1.2.0+ (HIVE-9118) – see LanguageManual DDL#Drop Table, LanguageManual DDL#Drop Partitions, LanguageManual DDL#Truncate Table, and Insert Overwrite.
TBLPROPERTIES ("EXTERNAL"="TRUE") in release 0.6.0+ (HIVE-1329) – Change a managed table to an external table and vice versa for "FALSE".
As of Hive 2.4.0 (HIVE-16324) the value of the property 'EXTERNAL' is parsed as a boolean (case insensitive true or false) instead of a case sensitive string comparison.
TBLPROPERTIES ("external.table.purge"="true") in release 4.0.0+ (HIVE-19981) when set on external table would delete the data as well.
To specify a database for the table, either issue the USE database\_name statement prior to the CREATE TABLE statement (in Hive 0.6 and later) or qualify the table name with a database name ("database\_name.table.name" in Hive 0.7 and later).
The keyword "default" can be used for the default database.
wget <https://raw.githubusercontent.com/hortonworks/data-tutorials/master/tutorials/hdp/how-to-process-data-with-apache-hive/assets/driver_data.zip>
unzip driver\_data.zip
CREATE TABLE training.drivers (
driverId INT,
name STRING,
ssn BIGINT,
location STRING,
certified STRING,
wageplan STRING)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
stored as textfile
Location '/user/cloudera/File\_directory/’
TBLPROPERTIES ("skip.header.line.count"="1");
create external TABLE training.timesheet (
driverId int,
week int,
hours\_logged int,
miles\_logged int)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
stored as textfile
Location '/user/cloudera/test\_supratik'
TBLPROPERTIES ("skip.header.line.count"="1");
CREATE TABLE training.truck (
driverId int,
truckId int,
eventTime string,
eventType string,
longitude string,
latitude string,
eventKey string,
CorrelationId string,
driverName string,
routeId string,
routeName string,
eventDate string)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
stored as textfile
Location '/user/cloudera/File\_directory/’
TBLPROPERTIES ("skip.header.line.count"="1");
**Managed and External Tables**
By default Hive creates managed tables, where files, metadata and statistics are managed by internal Hive processes.
**Managed tables**
A managed table is stored under the hive.metastore.warehouse.dir path property, by default in a folder path similar to /user/hive/warehouse/databasename.db/tablename/. The default location can be overridden by the location property during table creation. If a managed table or partition is dropped, the data and metadata associated with that table or partition are deleted. If the PURGE option is not specified, the data is moved to a trash folder for a defined duration.
**Use managed tables when Hive should manage the lifecycle of the table, or when generating temporary tables.**
**External tables**
An external table describes the metadata / schema on external files. External table files can be accessed and managed by processes outside of Hive. External tables can access data stored in sources such as Azure Storage Volumes (ASV) or remote HDFS locations. If the structure or partitioning of an external table is changed, an MSCK REPAIR TABLE table\_name statement can be used to refresh metadata information.
Storage Formats

Hive supports built-in and custom-developed file formats. 
The following are some of the formats built-in to Hive:

|     |     |
| --- | --- |
| Storage Format | Description |
| Storage Format | Description |
| STORED AS TEXTFILE | Stored as plain text files. TEXTFILE is the default file format, unless the configuration parameter hive.default.fileformat has a different setting.<br>Use the DELIMITED clause to read delimited files.<br><br>Enable escaping for the delimiter characters by using the 'ESCAPED BY' clause (such as ESCAPED BY '\\')<br>Escaping is needed if you want to work with data that can contain these delimiter characters.<br><br>A custom NULL format can also be specified using the 'NULL DEFINED AS' clause (default is '\\N'). |
| STORED AS SEQUENCEFILE | Stored as compressed Sequence File. |
| ==**STORED AS ORC**== | ==**Stored as [ORC file format](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC)**====**. Supports ACID Transactions & Cost-based Optimizer (CBO).**== Stores column-level metadata. |
| STORED AS PARQUET | Stored as Parquet format for the [Parquet](https://cwiki.apache.org/confluence/display/Hive/Parquet) columnar storage format in [Hive 0.13.0 and later](https://cwiki.apache.org/confluence/display/Hive/Parquet);<br>Use ROW FORMAT SERDE ... STORED AS INPUTFORMAT ... OUTPUTFORMAT syntax ... in [Hive 0.10, 0.11, or 0.12](https://cwiki.apache.org/confluence/display/Hive/Parquet). |
| STORED AS AVRO | Stored as Avro format in [Hive 0.14.0 and later](https://issues.apache.org/jira/browse/HIVE-6806) |
| STORED AS RCFILE | Stored as [Record Columnar File](https://en.wikipedia.org/wiki/RCFile) format. |
| STORED AS JSONFILE | Stored as Json file format in Hive 4.0.0 and later. |
| STORED BY | Stored by a non-native table format. To create or link to a non-native table, for example a table backed by HBase or [Druid](https://cwiki.apache.org/confluence/display/Hive/Druid+Integration) or Accumulo.<br>See StorageHandlers for more information on this option. |
| INPUTFORMAT and OUTPUTFORMAT | in the file\_format to specify the name of a corresponding InputFormat and OutputFormat class as a string literal.<br>For example, 'org.apache.hadoop.hive.contrib.fileformat.base64.Base64TextInputFormat'.<br>For LZO compression, the values to use are<br>'INPUTFORMAT "com.hadoop.mapred.DeprecatedLzoTextInputFormat"<br>OUTPUTFORMAT "org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat"' |

==HiveQL handles structured data only.== By default Hive has derby database to store the data in it. As mentioned HiveQL can handle only structured data. Data is eventually stored in files. There are some specific file formats which Hive can handle such as:
• TEXTFILE
• SEQUENCEFILE
• RCFILE
• ORCFILE
• Parquet file
• Avro File
File Format
==These file formats mainly varies between data encoding, compression rate, usage of space and disk I/O.==
==Hive does not verify whether the data that you are loading matches the schema for the table or not. However, it verifies if the file format matches the table definition or not.==
TEXTFILE
<https://cwiki.apache.org/confluence/display/Hive/FileFormats>
TEXTFILE format is a famous input/output format used in Hadoop. ==In Hive if we define a table as TEXTFILE it can load data of form CSV (Comma Separated Values), delimited by Tabs, Spaces and JSON data.== This means fields in each record should be separated by comma or space or tab or it may be JSON(Java Script Object Notation) data.
==By default if we use TEXTFILE format then each line is considered as a record.==
We can create a TEXTFILE format in Hive as follows:
CREATE TABLE base\_table\_bucket(key int, value int)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE;
At the end we need to specify the type of file format. If we do not specify anything it will consider the file format as TEXTFILE format.
The TEXTFILE input and TEXTFILE output format is present in the Hadoop package as shown below:            
org.apache.hadoop.mapred.TextInputFormat
org.apache.hadoop.mapred.TextOutputFormat
SEQUENCEFILE
We know that Hadoop’s performance is drawn out when we work with small number of files with big size rather than large number of files with small size. If the size of a file is smaller than the typical block size in Hadoop, we consider it as a small file. Due to this, the amount of metadata increases which will become an overhead to the NameNode. To solve this problem sequence files are introduced in Hadoop. Sequence files acts as a container to store the small files.
Sequence files are flat files consisting of binary key-value pairs. When Hive converts queries to MapReduce jobs, it decides on the appropriate key-value pairs to be used for a given record. Sequence files are in binary format which are able to split and the main use of these files is to club two or more smaller files and make them as a one sequence file.
In Hive we can create a sequence file by specifying STORED AS SEQUENCEFILE in the end of a CREATE TABLE statement.
There are three types of sequence files:
• Uncompressed key/value records.
• Record compressed key/value records – only ‘values’ are compressed here
• Block compressed key/value records – both keys and values are collected in ‘blocks’ separately and compressed. The size of the ‘block’ is configurable.
Hive has its own SEQUENCEFILE reader and SEQUENCEFILE writer for reading and writing through sequence files.
Hive uses the SEQUENCEFILE input and output formats from the following packages:
org.apache.hadoop.mapred.SequenceFileInputFormat
org.apache.hadoop.hive.ql.io.HiveSequenceFileOutputFormat
Creating SEQUENCEFILE
CREATE TABLE training.table\_sequence\_file(key int, value string)
row format delimited
fields terminated by '\\t'
stored as sequencefile;
Now to load data into this table is somewhat different from loading into the table created using TEXTFILE format. You need to insert the data from another table because this SEQUENCEFILE format is binary format. It compresses the data and then stores it into the table. If you want to load directly as in TEXTFILE format that is not possible because we cannot insert the compressed files into tables.
RCFILE
RCFILE stands of Record Columnar File which is another type of binary file format which offers high compression rate on the top of the rows.
RCFILE is used when we want to perform operations on multiple rows at a time.
RCFILEs are flat files consisting of binary key/value pairs, which shares much similarity with SEQUENCEFILE. RCFILE stores columns of a table in form of record in a columnar manner. It first partitions rows horizontally into row splits and then it vertically partitions each row split in a columnar way. RCFILE first stores the metadata of a row split, as the key part of a record, and all the data of a row split as the value part. This means that RCFILE encourages column oriented storage rather than row oriented storage.
This column oriented storage is very useful while performing analytics. It is easy to perform analytics when we “hive’ a column oriented storage
In Hive we can create a RCFILE format as follows:
CREATE TABLE training.table\_rcfile(key int, value string)
row format delimited
fields terminated by '\\t'
stored as RCfile;
insert INTO training.table\_rcfile  values(11, 100),(31,300),(21,200),(41,400),(61,600),(51,500);
Hive has its own RCFILE Input format and RCFILE output format in its default package:
org.apache.hadoop.hive.ql.io.RCFileInputFormat
org.apache.hadoop.hive.ql.io.RCFileOutputFormat
ORCFILE
ORC stands for Optimized Row Columnar which means it can store data in an optimized way than the other file formats. ORC reduces the size of the original data up to 75%. As a result the speed of data processing also increases. ORC shows better performance than Text, Sequence and RC file formats.
An ORC file contains rows data in groups called as Stripes along with a file footer. ORC format improves the performance when Hive is processing the data.
In Hive we can create a RCFILE format as follows:
CREATE TABLE training.table\_orc(key int, value string)
row format delimited
fields terminated by '\\t'
stored as ORC;
insert INTO training.table\_rcfile  values(11, 100),(31,300),(21,200),(41,400),(61,600),(51,500);
Hive has its own ORCFILE Input format and ORCFILE output format in its default package:
org.apache.hadoop.hive.ql.io.orc
AVRO FILE
AVRO is open source project that provides data serialization for compact binary format widely used for storing persistent data on HDFS. It is lightweight and has fast data serialisation and deserialization.
CREATE TABLE training.table\_avro(key int, value string)
row format delimited
fields terminated by '\\t'
stored as AVRO;
insert INTO training.table\_avro  values(11, 100),(31,300),(21,200),(41,400),(61,600),(51,500);
Hive has AVRO Input format and RCFILE output format in its default package:
org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat
org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat
Creating a Hive table requires schema definition first. And you can create a table with schema by either loading the schema from a file or by explicitly defining the schema in the command itself.
create an Avro backed Hive table by using an Avro schema file:
CREATE EXTERNAL TABLE training.customers\_avro
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.avro.AvroSerDe'
STORED AS avro location '/user/cloudera/customers\_avro'
TBLPROPERTIES ('avro.schema.url'='/user/cloudera/AutoGeneratedSchema.avsc');
The avro.schema.url is a URL (here a [file://](file:///) URL) pointing to an Avro schema file that is used for reading and writing, it could also be an hdfs
Parquet File Format:
Parquet file format is also a columnar format. Instead of just storing rows of data adjacent to one another you also store column values adjacent to each other. Just like ORC file, it’s great for compression with great query performance especially efficient when querying data from specific columns. Parquet format is computationally intensive on the write side, but it reduces a lot of I/O cost to make great read performance. It enjoys more freedom than ORC file in schema evolution, that it can add new columns to the end of the structure.
It supports both File-Level Compression and Block-Level Compression. File-level compression means you compress entire files regardless of the file format, the same way you would compress a file in Linux. Some of these formats are splittable (e.g. bzip2, or LZO if indexed). Block-level compression is internal to the file format, so individual blocks of data within the file are compressed. This means that the file remains splittable even if you use a non-splittable compression codec like Snappy. However, this is only an option if the specific file format supports it. Summary Overall these format can drastically optimize workloads, especially for Hive and Spark which tend to just read segments of records rather than the whole thing (which is more common in MapReduce). Since Avro and Parquet have so much in common when choosing a file format to use with HDFS, we need to consider read performance and write performance. Because the nature of HDFS is to store data that is write once, read multiple times, we want to emphasize on the read performance. The fundamental difference in terms of how to use either format is this: Avro is a Row based format. If you want to retrieve the data as a whole, you can use Avro. Parquet is a Column based format. If your data consists of lot of columns but you are interested in a subset of columns, you can use Parquet.
Additional Read
<https://blog.clairvoyantsoft.com/big-data-file-formats-3fb659903271>
File Usage,
a) If your data is delimited by some parameters then you can use TEXTFILE format.
b) If your data is in small files whose size is less than the block size then you can use SEQUENCEFILE format.
c) If you want to perform analytics on your data and you want to store your data efficiently for that then you can use RCFILE format.
d) If you want to store your data in an optimized way which lessens your storage and increases your performance then you can use ORCFILE format.
**Partitioned Tables**
Partitioned tables can be created using the PARTITIONED BY clause. A table can have one or more partition columns and a separate data directory is created for each distinct value combination in the partition columns. Each Table can have one or more partition Keys which determines how the data is stored. Partitions—apart from being storage units—also allow the user to efficiently identify the rows that satisfy a specified criteria.
**Buckets (or Clusters):**
Data in each partition may in turn be divided into Buckets based on the value of a hash function of some column of the Table. These can be used to efficiently sample the data. Tables or partitions can be bucketed using CLUSTERED BY columns, and data can be sorted within that bucket via SORT BY columns. This can improve performance on certain kinds of queries.
DO not include Partitioned column in the data of the table itself.
**Partitions:** Each Table can have one or more partition Keys which determines how the data is stored. Partitions—apart from being storage units—also allow the user to efficiently identify the rows that satisfy a specified criteria; for example, a date\_partition of type STRING and country\_partition of type STRING.
**Buckets (or Clusters):** Data in each partition may in turn be divided into Buckets based on the value of a hash function of some column of the Table. For example the page\_views table may be bucketed by userid, which is one of the columns, other than the partitions columns, of the page\_view table. These can be used to efficiently sample the data.
In hive, you create a table based on the usage pattern and so you should choose both partitioning the bucketing based on what your Analysis Queries would look like.
However, the following things are advisable
Partitioning
Partitioning helps you speed up the queries with predicates (i.e. Where conditions). So in your case, if city\_category is the field you are going to use most of the time in your where condition you should choose that field for partition.
It might degrade the performance of other queries.
Need to make sure that cardinality is not too high, otherwise, your query performance would be degraded.
To understand the above points you need to understand how partitioning works. When you create a partition (or subpartition), Hive creates a subfolder with that name and stores the data (files) into those folders.
So if you partition based on city\_category your file would look like this.
/data/table\_name/city\_category=A
/data/table\_name/city\_category=B
...
/data/table\_name/city\_category=E
This helps hive to find a particular record if you provide city\_category in Where condition as it has to just scan one folder.
However, if you try to find a record based on user\_id or product\_id then hive need to scan all the folders.
And let's say if you end up partitioning based on purchase\_amount, then you will have a lot many folders. NameNode has to maintain the location of each folder and files and so it will create a lot of load on your NameNode and obviously decrease the performance of your query.
**Bucketing**
It helps you in speeding up your join query if another table you are joining has similar bucketing.
However, it's a good idea to make sure data is distributed evenly in bucketing.
What bucketing does it, it applies a hashing on a given field and based on that it stores the given record in bucketing.
So let's say if you bucket based on city\_category and tell to create 50 buckets.
CLUSTERED BY (city\_category) INTO 50 BUCKETS
as we have only 5 categories, other 45 buckets would be empty, this is something you don't want as it will degrade the performance of your query.
Bucketing works well when the field has high cardinality and data is evenly distributed among buckets. Partitioning works best when the cardinality of the partitioning field is not too high.
How does Hive distribute the rows across the buckets? In general, the bucket number is determined by the expression hash\_function(bucketing\_column) mod num\_buckets. (There's a '0x7FFFFFFF in there too, but that's not that important). The hash\_function depends on the type of the bucketing column. For an int, it's easy, hash\_int(i) == i. For example, if user\_id were an int, and there were 10 buckets, we would expect all user\_id's that end in 0 to be in bucket 1, all user\_id's that end in a 1 to be in bucket 2, etc. For other datatypes, it's a little tricky. In particular, the hash of a BIGINT is not the same as the BIGINT. And the hash of a string or a complex datatype will be some number that's derived from the value, but not anything humanly-recognizable. For example, if user\_id were a STRING, then the user\_id's in bucket 1 would probably not end in 0. In general, distributing rows based on the hash will give you a even distribution in the buckets.
For the Int data type, the hash values are also an integer. However, for string or another complex data type, the hash value is computed basis each character present in the string or some other logic which is difficult to decode. Using a hashing algorithm for data distribution enables even distribution of data across buckets.

According to hash function :
6%3=0
3%3=0
**Static & Dynamic Partitions**
Static partitioning we need to specify the partition column value in each and every LOAD statement.
suppose we are having partition on column country for table t1(userid, name,occupation, country), so each time we need to provide country value
hive>LOAD DATA INPATH '/hdfs path of the file' INTO TABLE t1 PARTITION(country="US")
hive>LOAD DATA INPATH '/hdfs path of the file' INTO TABLE t1 PARTITION(country="UK")
dynamic partition allow us not to specify partition column value each time. the approach we follows is as below:
create a non-partitioned table t2 and insert data into it.
now create a table t1 partitioned on intended column(say country).
load data in t1 from t2 as below:
hive> INSERT INTO TABLE t2 PARTITION(country) SELECT \* from T1;
Make sure that partitioned column is always the last one in non-partitioned table(as we are having country column in t2)
**Joins:**
Only Equality joins are allowed In Joins
More than two tables can be joined in the same query
LEFT, RIGHT, FULL OUTER joins exist in order to provide more control over ON Clause for which there is no match
JOIN, Also referred as inner join The Records common to the both tables will be retrieved by this Inner Join.
LEFT OUTER JOIN: LEFT OUTER JOIN returns all the rows from the left table even though there are no matches in right table
If ON Clause matches zero records in the right table, the joins still return a record in the result with NULL in each columnfrom the right table
RIGHT OUTER JOIN:RIGHT OUTER JOIN returns all the rows from the Right table even though there are no matches in left table.RIGHT joins always return records from a Right table and matched records from the left table. If the left table is having novalues corresponding to the column, it will return NULL values in that place.
FULL OUTER JOIN:It combines records of both the tables sample\_joins and sample\_joins1 based on the JOIN Condition given in query.It returns all the records from both tables and fills in NULL Values for the columns missing values matched on either side.
LEFT SEMI JOIN, LEFT SEMI JOIN implements the uncorrelated IN/EXISTS subquery semantics in an efficient way.The restrictions of using LEFT SEMI JOIN are that the right-hand-side table should only be referenced in the join condition (ON-clause), but not in WHERE- or SELECT-clauses etc.
SELECT a.key, a.value
FROM a
WHERE a.key in
 (SELECT b.key
  FROM B);

An INNER JOIN returns the columns from both tables. A LEFT SEMI JOIN only returns the records from the left-hand table.
If there are multiple matching rows in the right-hand column, an INNER JOIN will return one row for each match on the right table, while a LEFT SEMI JOIN only returns the rows from the left table, regardless of the number of matching rows on the right side
CROSS JOIN. Cross join, also known as Cartesian product, is a way of joining multiple tables in which all the rows or tuples from one table are paired with the rows and tuples from another table. For example, if the left-hand side table has 10 rows and the right-hand side table has 13 rows then the result set after joining the two tables will be 130 rows. That means all the rows from the left-hand side table (having 10 rows) are paired with all the tables from the right-hand side table (having 13 rows).Cross join defines as a Cartesian product where the number of rows in the first table multiplied by the number of rows in the second table.If no where clause is used along with CROSS JOIN, this kind of result is called a Cartesian Product.
However, Hive only supports equal JOIN instead of unequal JOIN, because unequal JOIN is difficult to be converted to MapReduce jobs.
While performing JOIN, one shall keep bigger table (in terms of number of rows) on right side of join clause.
For example, table ‘A’ have 1 million rows and table B’’ have 10000 rows, then query should be like .... B join A ....;
Hive has two type of joins from MapReduce point of view joins performed in mapper and joins performed in reducer, map side join and reduce side join.
Hive has nature to perform joins in reducer phase. But this nature can be altered in some special cases to enhance query performance. Map side join can optimize query performance at considerable extent. MapJoin feature enables very fast joining by allowing a table to be loaded into memory. But table should be small enough to fit in memory. Configuration parameter hive.mapjoin.smalltable.filesize (default is 25MB) defines size of table to be cashed into memory. To perform MAPJOIN, smaller table must satisfy this condition.
Hive is trying to embrace CBO(cost based optimizer) in latest versions, and Join is one major part of it.
joins includes a map stage and a reduce stage.
•          Mapper:  reads the tables and output the join key-value pairs into an intermediate file.
•          Shuffle:   these pairs are sorts and merged.
•          Reducer: gets the sorted data and does the join.
The largest table should be put on the rightmost since it should be the stream table.
However you can use hint "STREAMTABLE" to change the stream table in each map-reduce stage.
Map Join(Broardcast Join)
If one or more tables are small enough to fit in memory, the mapper scans the large table and do the joins. No shuffle and reduce stage.
There are two ways to perform map side join, by using hint /\*+ MAPJOIN(smalltablename) \*/
Right/Full outer join don't work.
It requires at least one table is small enough.
The following is not supported.
Union Followed by a MapJoin
Lateral View Followed by a MapJoin
Reduce Sink (Group By/Join/Sort By/Cluster By/Distribute By) Followed by MapJoin
MapJoin Followed by Union
select /\*+ MAPJOIN(a) \*/ \* from user ‘a’ join orders ‘b’ on (a.name=b.order\_name);
and by setting hive.auto.convert.join to true and Hive will automatically use MapJoin for any tables smaller than hive.mapjoin.smalltable.filesize.
Mapjoins have a limitation in that the same table or alias cannot be used to join on different columns in the same query. (This makes sense because presumably Hive uses a HashMap keyed on the column(s) used in the join, and such a HashMap would be of no use for a join on different keys).
Bucket Map Join
Join is done in Mapper only. The mapper processing bucket 1 for table A will only fetch bucket 1 of table B.
![[Computing/Architectonic Pillars/System/Data Engineering/_resources/Hive.resources/unknown_filename.png]]

Use case:
When all tables are:
•          Large.
•          Bucketed using the join columns.
•          The number of buckets in one table is a multiple of the number of buckets in the other table.
•          Not sorted.
Cons:
Tables need to be bucketed in the same way how the SQL joins, so it cannot be used for other types of SQLs.
Tips:
1\. The tables need to be created bucketed on the same join columns and also data need to be bucketed when inserting.
One way is to set "hive.enforce.bucketing=true" before inserting data.
For example:
CREATE TABLE training.table\_1(key int, value string)
stored as TEXTFILE;
insert INTO table training.table\_1  values(1,'Anand12');
insert INTO table training.table\_1  values(1,'Anand12'),(1,'Anand13'),(2,'Anand14'),values(3,'Sam12'),values(4,'SAM13');
insert INTO table training.table\_1  values(1,'Anand13');
insert INTO table training.table\_1  values(2,'Anand14');
insert INTO table training.table\_1  values(3,'Sam12');
insert INTO table training.table\_1  values(4,'SAM13');
CREATE TABLE training.table\_2(key int, value string)
stored as TEXTFILE;
insert INTO training.table\_2  values(1,'Anand12'),(1,'Anand12'),(1,'Anand13'),(2,'Anand14'),(2,'Sam12'),(4,'SAM12');
insert INTO training.table\_2  values(1,'Anand12');
insert INTO training.table\_2  values(1,'Anand13');
insert INTO training.table\_2  values(2,'Anand14');
insert INTO training.table\_2  values(2,'Sam12');
insert INTO training.table\_2  values(4,'SAM12');
SELECT \*
FROM table\_1 a
INNER JOIN table\_2 b ON a.key=b.key;
SELECT \*
FROM table\_1 a
    JOIN table\_2 b ON a.key=b.key;
SELECT \*
FROM table\_1 a
    LEFT outer JOIN table\_2 b ON (a.key=b.key);
SELECT \*
FROM table\_1 a
    RIGHT outer JOIN table\_2 b ON (a.key=b.key);
SELECT \*
FROM table\_1 a
Full outer JOIN table\_2 b ON (a.key=b.key);
SELECT \*
FROM table\_1 a
 cross JOIN table\_2 b ON (a.key=b.key)
SELECT \*
FROM table\_1 a
LEFT SEMI JOIN table\_2 b ON (a.key=b.key)
"cross Joins and Cartesian Products with the CROSS JOIN Operator"
create table heroes (name string, era string, planet string);
create table villains (name string, era string, planet string);
insert into heroes values
('Tesla','20th century','Earth'),
('Pythagoras','Antiquity','Earth'),
('Zopzar','Far Future','Mars');
insert into villains values
('Caligula','Antiquity','Earth'),
('John Dillinger','20th century','Earth'),
('Xibulor','Far Future','Venus');
select concat(heroes.name,' vs. ',villains.name) as battle
from heroes join villains
where heroes.era = villains.era and heroes.planet = villains.planet;
select concat(heroes.name,' vs. ',villains.name) as battle from heroes cross join villains;select concat(heroes.name,' vs. ',villains.name) as battle
from heroes join villains
where heroes.era = villains.era and heroes.planet = villains.planet;
select concat(heroes.name,' vs. ',villains.name) as battle from heroes cross join villains;
select /\*+ MAPJOIN(heroes) \*/ concat(heroes.name,' vs. ',villains.name) as battle
from heroes join villains
where heroes.era = villains.era and heroes.planet = villains.planet;
select /\*+ STREAMTABLE(villains) \*/ concat(heroes.name,' vs. ',villains.name) as battle
from heroes join villains
where heroes.era = villains.era and heroes.planet = villains.planet;
select concat(heroes.name,' vs. ',villains.name) as battle
from heroes join villains
where heroes.era = villains.era and heroes.planet = villains.planet;
set hive.optimize.bucketmapjoin=true;
select /\*+ MAPJOIN(heroes) \*/ concat(heroes.name,' vs. ',villains.name) as battle from heroes join villains
select concat(heroes.name,' vs. ',villains.name) as battle
from heroes join villains
select concat(heroes.name,' vs. ',villains.name) as battle
from heroes join villains
where heroes.era = villains.era and heroes.planet = villains.planet;
**Order, Sort, Cluster, and Distribute By**
Default sorting order is ascending. default null sorting order for ASC order is NULLS FIRST, while the default null sorting order for DESC order is NULLS LAST.
In the strict mode (i.e., hive.mapred.mode=strict), the order by clause has to be followed by a "limit" clause. The limit clause is not necessary if you set hive.mapred.mode to nonstrict. The reason is that in order to impose total order of all results, there has to be one reducer to sort the final output. If the number of rows in the output is too large, the single reducer could take a very long time to finish.
Hive uses the columns in SORT BY to sort the rows before feeding the rows to a reducer. The sort order will be dependent on the column types. If the column is of numeric type, then the sort order is also in numeric order.
Hive supports SORT BY which sorts the data per reducer. The difference between "order by" and "sort by" is that the former guarantees total order in the output while the latter only guarantees ordering of the rows within a reducer. If there are more than one reducer, "sort by" may give partially ordered final results.
Cluster, and Distribute By in SELECT statements if there is a need to partition and sort the output of a query for subsequent queries.
Cluster By is a short-cut for both Distribute By and Sort By. Hive uses the columns in Distribute By to distribute the rows among reducers. All rows with the same Distribute By columns will go to the same reducer. However, Distribute By does not guarantee clustering or sorting properties on the distributed keys. Instead of specifying Cluster By, the user can specify Distribute By and Sort By, so the partition columns and sort columns can be different.
Cluster By is a short-cut for both Distribute By and Sort By.
**UNION**
UNION is used to combine the result from multiple SELECT statements into a single result set.
Hive 1.2.0 only support UNION ALL, in which duplicate rows are not eliminated.
Hive 1.2.0 and later, the default behavior for UNION is that duplicate rows are removed from the result. The optional DISTINCT keyword has no effect other than the default because it also specifies duplicate-row removal. With the optional ALL keyword, duplicate-row removal does not occur and the result includes all matching rows from all the SELECT statements.
insert overwrite table table\_5
select key, value from table\_1
union all
select key, value from table\_2;
insert overwrite table table\_6
select \* from table\_1
union all
select \* from table\_2;
**GROUP BY & Having**
GROUP BY clause explicitly groups data. Hive supports implicit grouping, which occurs when aggregating(AVG, SUM, MAX,MIN,COUNT) the table in full. Multiple aggregations can be done at the same time, however, no two aggregations can have different DISTINCT columns.
When using group by clause, the select statement can only include columns included in the group by clause, you can have as many aggregation functions (e.g. count) in the select statement
select key,value from table\_1 group by key; Does not work select clause has an additional column (b) that is not included in the group by clause (and it's not an aggregation function )
hive.map.aggr controls how we do aggregations.Default is false.
set hive.map.aggr=true; Hive will do the first-level aggregation directly in the map task.
This usually provides better efficiency, but may require more memory to run successfully.
The HQL HAVING clause is used with GROUP BY clause. Its purpose is to apply constraints on the group of data produced by GROUP BY clause. Thus, it always returns the data where the condition is TRUE.
The GROUP BY clause comes after the WHERE clause and before the ORDER BY clause.
Grouping columns can be column names or derived columns.
Every nonaggregate column in the SELECT clause must appear in the GROUP BY clause.
**ROW\_NUMBER, RANK and DENSE\_RANK Analytical Functions**
Row\_number: Assign unique values to each row or rows within group based on the column values used in OVER clause.
Rank: Get rank of the rows in column or within group. Rows with equal values receive the same rank with next rank value skipped.
Dense rank: Get rank of a value in a group. Rows with equal values for ranking criteria receive the same rank and assign rank in sequential order.
select key,value,row\_number() over(order by key) as rn,
rank() over(order by key) as rank,dense\_rank() over(order by key) as denserank from table\_6;
Analytic functions and aggregate functions consist of an over clause which helps in defining the partitions and the sorting of data within each partition.
This Over clause defines a window or user-specified set of rows within a query result set. Based on the defined window, the aggregate function is calculated.
**First and Last**
First\_Value and Last\_Value in Hive or Spark SQL is used obtain the first value and last value from all the rows in a table or from a specified window.
Syntax: first\_value(column/expression) over(partition by col(s)/expression(s) | order by col(s)/expression(s))
Syntax: last\_value(column/expression) over(partition by col(s)/expression(s) | order by col(s)/expression(s))
select \*, first\_value(marks) over(partition by dept order by amount desc) as first\_value\_marks , last\_value(marks) over(partition by dept order by amount desc) as last\_value\_marks from training.dataset.
**Lead and Lag**
As the name suggests, lead and lag are used to fetch the next and previous record respectively from the group of rows.
Syntax: lead(column/expression, offset, default\_value) over(partition by col(s)/expression(s) | order by col(s)/expression(s))
Note: Offset and default\_value is optional. By default, offset is 1 and default\_value is null
**Execution Engine**
Execution engine. Options are: mr (Map Reduce, default), tez (Tez execution, for Hadoop 2 only), or spark (Spark execution, for Hive 1.1.0 onward).
While mr remains the default engine for historical reasons, It may be removed without further warning.
Tez is a new application framework built on Hadoop Yarn that can execute complex directed acyclic graphs of general data processing tasks. In many ways it can be thought of as a more flexible and powerful successor of the map-reduce framework.
Tez also greatly extends the possible ways of which individual tasks can be linked together; In fact any arbitrary DAG can be executed directly in Tez.
For more information
See Hive on Tez: <https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez>
See Hive on Spark:
<https://cwiki.apache.org/confluence/display/Hive/Hive+on+Spark%3A+Getting+Started>
Using Tez/Spark as Execution Engine
In a nutshell, Spark is a more mature version of Tez, plus much much more CBOwhere data volume is not too large but computation is very highly CPU intensive, Spark is best way to go.
Tez has proven itself at large scale workloads, in high volume of data.
**Explode**
explode () takes in an array (or a map) as an input and outputs the elements of the array (map) as separate rows. UDTFs can be used in the SELECT expression list and as a part of LATERAL VIEW.
As an example of using explode() in the SELECT expression list, consider a table named myTable that has a single column (myCol) and two rows:
\[100,200,300\]
\[400,500,600\]
Then running the query:
SELECT explode(myCol) AS myNewCol FROM myTable;
will produce:
100
200
300
400
500
600
posexplode() is similar to explode but instead of just returning the elements of the array it returns the element as well as its position in the original array.
As an example of using posexplode() in the SELECT expression list, consider a table named myTable that has a single column (myCol) and two rows:

|     |
| --- |
| Array<int> myCol |
| \[100,200,300\] |
| \[400,500,600\] |

Then running the query:
SELECT posexplode(myCol) AS pos, myNewCol FROM myTable;
will produce:

|     |     |
| --- | --- |
| (int) pos | (int) myNewCol |
| (int) pos | (int) myNewCol |
| 1   | 100 |
| 2   | 200 |
| 3   | 300 |
| 1   | 400 |
| 2   | 500 |
| 3   | 600 |

**Optimization techniques**

* Partitioning/Bucketing
* Bucketing
* Using Tez as Execution Engine
* Using Compression
* Using ORC Format
* Join Optimizations
* Cost-based Optimizer

Partitioning/Bucketing
When we query data on a partitioned table, it will only scan the relevant partitions to be queried and skips irrelevant partitions. Now, assume that even on partitioning, the data in a partition was quite big, to further divide it into more manageable chunks we can use Bucketing.
Compression
hive queries involve a lot of Disks I/O or Network I/O operations, which can be easily reduced by reducing the size of the data which is done by compression.
Following are the main situations where I/O operations are performed and compression can save cost:

* Reading data from a local DFS directory
* Reading data from a non-local DFS directory
* Moving data from reducers to the Next stage Mappers/Reducers
* Moving the final output back to the DFS.

Also, DFS replicates the data as well to be fault-tolerant, there are more I/O operations involved when we are replicating data.
Using ORC Format
Choosing the correct file format can be another thing which can optimize the processing job to a great extent.
ORC files enable numerous optimizations in Hive like the following:
Achieves higher level of compression, the compression algorithm can be changed using [orc.compress](http://orc.compress/) setting in the hive. By default, it uses Zlib Compression.
It has the ability to skip scanning an entire range of rows within a block, if irrelevant to the query, using the light-weight indexes stored within the file.
It has the ability to skip decompression of rows within a block, if irrelevant to the query.
Single file as the output of each task, reducing the load on the name node.
It supports multiple streams to read the file simultaneously.
It keeps the metadata stored in the file using Protocol Buffers, used for serializing structured data.
The ORC storage strategy is both column-oriented and row-oriented. An ORC format file consists of tabular data that is written in “stripes” of a convenient size to process in a single mapper (256 MB and 512 MB are common sizes.) Stripes may or may not be processed by separate tasks, and a single ORC file can contain many stripes, each of which is independent. In stripes, the columns are compressed separately, only the columns that are in the projection for a query need be decompressed.
Within a stripe the data is divided into 3 Groups:
The stripe footer contains a directory of stream locations.
Row data is used in table scans, by default contains 10,000 rows.
Index data include min and max values for each column and row positions within each column. Row index entries provide offsets that enable seeking to the right compression block and byte within a decompressed block. Note that ORC indexes are used only for the selection of stripes and row groups and not for answering queries.
Just using ORC format, gives rise to the following optimizations:
Push-down Predicates
Bloom Filters
ORC Compression
Vectorization
Push-down Predicates
The ORC reader uses the information present in the Index data of the stripes, to make sure whether the data in the stripe is relevant to the query or not. So, essentially the where conditions of the query are passed to the ORC Reader, before bothering to unpack the relevant columns from the row data block. For instance, if the value from the where condition is not within the min/max range for a block, the entire block can be skipped.
Bloom Filters
Bloom Filters is a probabilistic data structure that tells us whether an element is present in a set or not by using a minimal amount of memory. A catchy thing about bloom filters is that they will occasionally incorrectly answer that an element is present when it is not. Their saving grace, however, is that they will never tell you that an element is not present if it is.
Bloom Filters again helps in the push-down predicates for ORC File formats. If a Bloom filter is specified for a column, even if the min/max values in a row-group’s index say that a given column value is within the range for the row group, the Bloom filter can answer specifically whether the value is actually present. If there is a significant probability that values in a where clause will not be present, this can save a lot of pointless data manipulation.
We can set the bloom filter columns and bloom filter’s false positive probability using the following table properties:
orc.bloom.filter.columns: comma-separated list of column names for which bloom filter should be created
orc.bloom.filter.fpp: false positive probability for bloom filter.
ORC Compression
ORC format not only compresses the values of the columns but also compresses the stripes as a whole. It also provides choices for the overall compression algorithm. It allows the following compression algorithm to be used :
Zlib: Higher Compression. Uses more CPU and saves space.
Snappy : Compresses less. Uses less CPU and more space.
None: No compression at all.
If the CPU is tight, you might want to use Snappy. Rarely will you want to use none, which is used primarily for diagnostic purposes. Also, the Hive’s writer is intelligent if it does not foresee any marginal gain, it will not compress the data.
Vectorization
Query Operations like scanning, filtering, aggregations, and joins can be optimized to a great extent using the vectorized query execution. A standard query execution processes one row at a time and goes through a long code path as well as significant metadata interpretation.
Vectorization optimizes the query execution by processing a block of 1024 rows at a time, inside which each row is saved as a vector. On vectors, simple arithmetic and comparison operations are done very quickly. Execution is speeded up because within a large block of data of the same type, the compiler can generate code to execute identical functions in a tight loop without going through the long code path that would be required for independent function calls. This SIMD-like behavior gives rise to a host of low-level efficiencies: fewer instructions executed, superior caching behavior, improved pipelining, more favorable TLB behavior, etc.
Not every data type and operation can benefit from vectorization, but most simple operations do, and the net is usually a significant speedup.
To use vectorized query execution, you must store your data in ORC format, and set the following variable as shown in Hive SQL:
set hive.vectorized.execution.enabled = true;
set hive.vectorized.execution.reduce.enabled=true
set hive.vectorized.execution.reduce.groupby.enabled=true
We can check if the query is being executed in a vectorized manner using the Explain Command. In the output of the Explain Command, you will probably see the following in different stages:
Execution mode: vectorized
Join Optimizations
The join query is the table which is mentioned last is streamed through the reducers, rest all are buffered in the memory in the reducers. So, we should always keep in mind that the large table is mentioned in the last which will lessen the memory needed by the reducer, or we can use the STREAMTABLE.
Another thing to pay attention to when where clause is used with a join, then join part is executed first and then the results are filtered using the where clauses. Like, in the below query:
SELECT a.val, b.val FROM a LEFT OUTER JOIN b ON (a.key=b.key) WHERE a.ds=’2009–07–07' AND b.ds=’2009–07–07'
While the same filtering of the records could have been executed along when joining the tables. This can be done by including the filtering conditions along with the join conditions.
SELECT a.val, b.val FROM a LEFT OUTER JOIN b
ON (a.key=b.key AND b.ds=’2009–07–07' AND a.ds=’2009–07–07')
Map Join (Broadcast Join or Broadcast-Hash Join)
Useful for star schema joins, this joining algorithm keeps all of the small tables (dimension tables) in memory in all of the mappers and big table (fact table) is streamed over it in the mapper. This avoids shuffling cost that is inherent in Common-Join. For each of the small table (dimension table), a hash table would be created using join key as the hash table key and when merging the data in the Mapper Function, data will be matched with the mapping hash value.
Bucket Map Join
Let’s assume that the size of the tables bigger to fit in the memory of the Mapper. But when chunked into buckets can fit in the memory, the tables being joined are bucketized on the join columns, and the number of buckets in one table is a multiple of the number of buckets in the other table, the buckets can be joined with each other by creating a hash table of the smaller table’s bucket in the memory and streaming the other table’s content in the mapper. Assuming, If table A and B have 4 buckets each, the following join can be done on the mapper only.
Instead of fetching B completely for each mapper of A, only the required buckets are fetched. For the query above, the mapper processing bucket 1 for A will only fetch bucket 1 of B. It is not the default behavior, and is governed by the following parameter.
set hive.optimize.bucketmapjoin = true
Cost-based Optimizations
The CBO lets hive optimize the query plan based on the metadata gathered. CBO provides two types of optimizations: logical and physical.
Logical Optimizations:
Projection Pruning
Deducing Transitive Predicates
Predicate Pushdown
Merging of Select-Select, Filter-Filter into a single operator
Multi-way Join
Query Rewrite to accommodate for Join skew on some column values
Physical Optimizations:
Partition Pruning
Scan pruning based on partitions and bucketing
Scan pruning if a query is based on sampling
Apply Group By on the map side in some cases
Optimize Union so that union can be performed on map side only
Decide which table to stream last, based on user hint, in a multiway join
Remove unnecessary reduce sink operators
For queries with limit clause, reduce the number of files that needs to be scanned for the table.
Turning on CBO will typically halve the execution time for a query, and may do much better than that. Along with these optimizations, CBO will also transform join queries into the best possible join using the statistics of the table saved in the metastore.
CBO can be turned on using the following setting in the hive-site.xml file :
hive.cbo.enable=true
The statistics of the table is auto gathered by hive, if hive.stats.autogather is set to True else could be calculated using the ANALYZE command.
**Insert, Update, Delete & Merge**
[https://docs.cloudera.com/HDPDocum](https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.6.0/bk_data-access/content/new-feature-insert-values-update-delete.html)[ents/HDP2/HDP-2.6.0/bk_data-access/content/new-feature-insert-values-update-delete.html](https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.6.0/bk_data-access/content/new-feature-insert-values-update-delete.html)
**Apache Impala.**
Impala keeps its table definitions in a traditional MySQL or PostgreSQL database known as the metastore, the same database where Hive keeps this type of data. Thus, Impala can access tables defined or loaded by Hive, as long as all columns use Impala-supported data types, file formats, and compression codecs.
For DDL and DML issued through Hive, or changes made manually to files in HDFS, you still use the REFRESH statement (when new data files are added to existing tables) or the INVALIDATE METADATA statement (for entirely new tables, or after dropping a table, performing an HDFS rebalance operation, or deleting data files). Issuing INVALIDATE METADATA by itself retrieves metadata for all the tables tracked by the metastore. If you know that only specific tables have been changed outside of Impala, you can issue REFRESH table\_name for each affected table to only retrieve the latest metadata for those tables.
Hive is only for ETLs and batch-processing.Analysts will get their answer way faster using Impala, although unlike Hive, Impala is not fault-tolerance. Impala uses MPP (Massive Parallel Processing) engine.
Impala integrates with the Apache Hive metastore database, to share databases and tables between both components. The high level of integration with Hive, and compatibility with the HiveQL syntax, lets you use either Impala or Hive to create tables, issue queries, load data, and so on.
key advantages of Impala:
Impala can access data directly from the HDFS file system.
Impala returns results typically within seconds or a few minutes, rather than the many minutes or hours that are often required for Hive queries to complete.
Impala is faster than Hive because it’s a whole different engine and Hive is over MapReduce.
Impala is pioneering the use of the Parquet file format, a columnar storage layout that is optimized for large-scale queries typical in data warehouse scenarios.
SparkSQL is much faster than Hive, especially if it performs only in-memory computations, but Impala is still faster than SparkSQL.
It’s faster because Impala is an engine designed especially for the mission of interactive SQL over HDFS, and it has architecture concepts that helps it achieve that. For example the Impala ‘always-on’ daemons are up and waiting for queries 24/7 — something that is not part of SparkSQL. And some more reasons like Impala’s codegen mechanism, the Parquet format optimization, statistics, metadata cache, etc.
**Impala Best Practices**
Use The Parquet Format.
Impala performs best when it queries files stored as Parquet format.
Work With Partitions
Partition your data according to your analysts queries. Impala doesn’t have any indexes so that’s the only way to reduce the amount of data you process in each query. Over partitioning can be dangerous (keep reading for more details).
The REFRESH Statement instead of Invalidate metadata.
 

Source

1. <http://hortonworks.com/wp-content/uploads/2016/05/Hortonworks.CheatSheet.SQLtoHive.pdf>
2. <https://medium.com/edureka/hive-commands-b70045a5693a>
3. <https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.1.5/migrating-data/content/hive_data_migration.html>
4. <https://dam.ukdataservice.ac.uk/media/604456/hiveworkshoppractical.pdf>
5. <https://docs.aws.amazon.com/emr/latest/ReleaseGuide/EMR_Interactive_Hive.html>
