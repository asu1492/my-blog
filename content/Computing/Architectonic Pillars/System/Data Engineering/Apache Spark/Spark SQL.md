---
---
What ?

1. Spark module for structured data processing 
2. execute SQL queries on Spark DF
3. Provides a programming abstraction called Dataframes and can also act as a distributed SQL query engine 
4. uses Spark Engine that scales to 1000s of nodes and multi hour queries
5. usable in Java, Scala, Python and R

Benefits

* cost based optimizer 
* columnar storage 
* code generation to make queries fast 

Spark SQL Data Sources

1. Parquet files 
	1. reading, writing and preserving data schema 
	2. can also run queries without loading the file 
2. JSON Datasets : Spark infers the schema and loads the dataset as a DataFrame 
3. R/W data stored in Hive Tables  

Running SQL Query

1. Requires creating a table view(programmatically on a DF), a temporary table to run SQL queries 
	1. Temporary view --- local scope withing current Spark Session 
	2. Global Temp view -- global scope within spark application -- shareable across diff spark sesion 
2. Creating a View using pyspark 
	1. Creating DF from a file : df = spark.read.json('people.json')
	2. Create a temp view : df.createTempView('people') || df.createGlobalTempView("people")
	3.  Run SQL query       : spark.sql("Select \* FROM people").show() || spark.sql("SELECT \* FROM global\_temp.people").show()
3. Aggregation Data 
	1. DF has inbuilt commonly used aggregation functions - count(), countDistinct(), avg(), max(), min(), and others. 
	2. Aggregation using SQL queries and tableview 
	3. ![[Computer Science/Data Engineering/Apache Spark/_resources/Spark_SQL.resources/image (2).png]]
	4. sdf.select('column').show(5)

SPARK SQL Optimization 

1. primary objective is to reduce query time and memory consumption, saving org time and money 
2. Catalyst
	1. is Spark SQL built in rule based query optimizer 
	2.  based on Scala's functional programming constructs
	3. easily adds new optimization techniques and features 
	4. enables developers to add data source specific rules and supports new data types 
	5. Catalyst optimizer uses tree data structure and set of rules 
	6. catalyst performs following 4 high level tasks for query execution 
		1. Analysis - USE OF catalyst to create a logical plan 
		2. Logical Optimization - evolution of LP to Optimized logical plan. OLP is rule based optimization and rules such as folding, push down and pruning are applied.  
		3. Physical Planning -- based on OLP. Multiple PP are generated with computation requirement. PP with least cost is than given go ahead. 
		4. Code Generation - Java byte code is generated on selected Physical Plan.  
	7. ![[Computer Science/Data Engineering/Apache Spark/_resources/Spark_SQL.resources/image (3).png]]
3. Rule based optimization 
	1. defines how to run the query 
	2. e.g. is table indexed, does query contain only the required columns ? 
4. Cost based optimization 
	1. time and memory a query consumes 
	2. What are the best paths for multiple datasets to use when querying data ? 
5. Tungsten 
	1. It's a spark's cost based optimization 
	2. it maximizes CPU and memory performance 
	3. Optimizes JVM for data processing, initially designed for transactions 
	4. Manages memory explicitly and does not rely on JVM object model or garbage collection. 
	5. Cache friendly computation of data structure and algorithms using STRIDE based memory access instead of RAM 
	6. Supports on demand JVM byte code generation 
	7. does not enable virtual function dispatches 
	8. places intermediate data in CPU registers and enables loop unrolling 

Memory Optimization
