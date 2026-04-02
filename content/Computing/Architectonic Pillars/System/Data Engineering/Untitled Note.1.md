---
---
Credit card data analysis
client -- postgreSQL -- HBASE (schema)

staging tables
  deduplication query on staging tables

Bigdata tools

* Hive
* Sqoop
* Spark
* HBase

Sqoop
  What is sqoop
  Why sqoop is used
       import and export of data from RBBMS to HDFS and vice versa
  Incremental
       append and last modified
            Append : new rows
            Last Modified : insert and update (just like upsert)
  Importing table without primary key
       by using splitby
  Arguments for sqoop import command
  Warehouse directory and target directory 
Spark
    Version of Spark used

    Used with
     python
    Why Spark??
         100x faster than mapreduce
         independent of hdfs
    What are ?
         tranformation -- e.g. filter, map
         Action
    Diff b/w RDD, Dataframe and Dataset
         RDD - collection of java objects distributed across various m/c
         resilient -- new RDD created in case of transformation
         non structured query 
         laziness, fault tolerance
    How to create RDD
         sc.parallelize
         sc.text
       Dataframe -- RDD(non-strucutred data)+Schema( enable SQL queries)
       DataSET -- map function -- memory optimization
    How to import data in Spark
    Can u use incremental import in Spark
    Data injection in Spark
    How do you run spark import job
    Spark Submit Command
         /usr/bin/spark-submit --jars --master yarn --deploy-mode client --driver-memory 3g --num-executors 1 --executor-cores 1 --executor-memory 1g
         /usr/bin/spark/spark-submit --jars /home/Hadoop/dep/PostgreSQL --master yarn --deploy-mode client --driver -memoru 3g --executer -momory 2g filename.py
    Incremental input or full load
    How did you run spark job -- sparkesummit command -- through putty  
    Sparksummit command :
    No. of executors and cores in Spark
         Yarn resource manager -- 1 core
    Deployment Mode
         Client Mode and Cluster Mode
    Command for dataframe - spark.read.format.csv
    renaming column name in dataframe
    coalesce and repartition
    how to create dataframe/RDD
    Created dataframe and now viewing first 100 records ?
    count of dataframe
    print schema of dataframe
    diff b/w map and flatmap function
    Optimization
      Spark follows memory computation -- if data does not fit in memory, then we use persist
      Cache/Persist
      Persist
           Memory
           Memory + Disk
    Transformation
         Narrow
         Wide : Shuffle partition
    Errors
         Java Heapspace
    Submit spark command : -username -password

Diff file formats used in HDFS
   CSV
   ORC
   Parquet
   Json
   Avro
   diff b/w csv, text and 
Volume of data you worked on
Adv f using ORC format
     greater compression
Optimization technique in Hive
  Joins
  Parititioning and bucketing
Hive
What is hive
     Query lang
Why hive used
Types of table in Hive
  External table
  Internal table
Why ?
  OLTP
Execution engine
  TEZ : Hive optimization technique
  Mapreduce
       Node Manager -- launched application master in datanode
       Application master -- data location -- node manager infor about datas
       resources req to Node Manager
       Task
Optimization techniques in HIVE
     Indexing
     ORC file format -- compresses to higher level
     Parititioning and bucketing
            Diff
            when to use each of them
             P: posibility of duplicate data
             B: in case f unique data
     Vectorization
             1024 records in one batch -- speeds up the process
            How to enable vectorization -- set property : true
       Cost based optimization
Types of Partition
   static
   dynamic
  P done based on column value

HBase
1\. NO SQL databases
2\. define column families
