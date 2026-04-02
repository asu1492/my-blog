---
source: https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.1.5/using-hiveql/content/hive_create_an_external_table.html
---
# Create, use, and drop an external table

You use an ==external table, which is a table that Hive does not manage, to import data from a file on a file system, into Hive.== In contrast to the Hive managed table, an external table ==keeps its data outside the Hive metastore.== H==ive metastore stores only the schema metadata of the external table.== Hive does not manage, or restrict access, to the actual external data.

In t==his task, y====ou need access to HDFS to put a comma-separated values (CSV) file on HDFS.== If you do not use Ranger and an ACL is not in place that allows you to access HDFS, you need to log in to a node on your cluster as the hdfs user. Alternatively, when using Ranger, you need to be authorized by a policy, such as the default HDFS all-path policy (shown below) to access HDFS.

![[hive_all_path_policy.png]]

In this task, you create an ==external table from CSV (comma-separated values) data stored on the file system,== depicted in the diagram below. ==Next, you want Hive to manage and store the actual data in the metastore. You create a managed table.==

![[hive-tables-managed.png]]

You insert the external table data into the managed table.

This task demonstrates the following Hive principles:

		Specifying a database location in the CREATE DATABASE command, for example `C==REATE DATABASE <managed table db name> LOCATION '<path>'==` ==works for managed tables only.== To specify the location of an external table, you need to include the specification in the table creation statement as follows:
	
	    CREATE EXTERNAL TABLE my_external_table (a string, b string)  
	                            ROW FORMAT SERDE 'com.mytables.MySerDe' 
	                            WITH SERDEPROPERTIES ( "input.regex" = "*.csv")
	                            LOCATION '/user/data';       
	

* The LOCATION clause in the CREATE TABLE specifies the location of external (not managed) table data.

* External table drop: Hive drops only the metadata, consisting mainly of the schema.

* Managed table drop: Hive deletes the data and the metadata stored in the Hive warehouse.

After dropping an external table, the ==data is not gone. To retrieve it, you issue another CREATE EXTERNAL TABLE statement to load the data from the file system.==

1. Create a text file named students.csv that contains the following lines.
	
	    1,jane,doe,senior,mathematics
	    2,john,smith,junior,engineering               
	
	* On the command-line of a node on your cluster, enter the following commands:
		
		    sudo su -
		    mv students.csv /home/hdfs
		    sudo su - hdfs
		    hdfs dfs -mkdir /user/andrena
		    hdfs dfs -chmod 777 /user/andrena
		    hdfs dfs -put /home/hdfs/students.csv /user/andrena
		    hdfs dfs -chmod 777 /user/andrena/students.csv
		

* Having authorization to HDFS through a Ranger policy, use the command line or Ambari to create the directory and put the students.csv file in the directory.

* Start the Hive shell as user max. For example, ==substitute the URI of your HiveServer:== `==beeline -u jdbc:hive2://myhiveserver.com:10000 -n max==`
* Create an external table schema definition that specifies the text format, loads data from students.csv located in /user/andrena.
	
	    CREATE EXTERNAL TABLE IF NOT EXISTS names_text(
	      student_ID INT, FirstName STRING, LastName STRING,    
	      year STRING, Major STRING)
	      COMMENT 'Student Names'
	      ROW FORMAT DELIMITED
	      FIELDS TERMINATED BY ','
	      STORED AS TEXTFILE
	      LOCATION '/user/andrena';
	
* Verify that the Hive warehouse stores the student names in the external table. `SELECT * FROM names_text;`
* Create the schema for the managed table to store the data in Hive metastore.
	
	    CREATE TABLE IF NOT EXISTS Names(
	      student_ID INT, FirstName STRING, LastName STRING,    
	      year STRING, Major STRING)
	      COMMENT 'Student Names';
	
* Move the external table data to the managed table.`INSERT OVERWRITE TABLE Names SELECT * FROM names_text;`
* Verify that the data now resides in the managed table also, drop the external table metadata, and verify that the data still resides in the managed table.
	
	    SELECT * from Names; 
	    DROP TABLE names_text;
	    SELECT * from Names;                  
	
	The results from the managed table Names appears.
	
* Verify that the external table schema definition is lost. `SELECT * from names_text;`
	Selecting all from `names_text` returns no results because the external table schema is lost.
	
* Check that the students.csv file on HDFS remains intact.

**Parent topic:** [Apache Hive 3 tables](https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.1.5/using-hiveql/content/hive_hive_3_tables.html)
