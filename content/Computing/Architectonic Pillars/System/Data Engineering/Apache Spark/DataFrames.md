---
---
* Equivalent to a table in relational database or data frame in R/Python, but with richer optimization. 
* Built on the top of RDD API
* They hold data in column & row format 
	* each row rep an individual data point. 
	* each column rep some feature or variable 
* Spark 2.0 shifted to dataframe sytax, much easier and cleaner than RDD syntax(used by Spark 1.0). 
* We can carry out various transformation on data using DF. 

Benefits

1. Scalabale -Kb to Pb
2. Support wide array of data formats and storage systems
3. State of the art optimization and code generation through the Spark SQL Catalyst Optimizer 
4. Seamless integration with all big data tooling and infra via spark  

DF Operations || Also know as [[ETL]] operations 

1. Reading : read the data 
2. Analysis : analyze the data -- examining columns, datatypes, no of rows, aggregated stats 
3. Transform the data -- sorting, joining ; 
4. Loading transformed data and writing it back to the disk 

Read the data

1. Create a DF or create a DF from an existng DF
	1. import pandas as pd
	2. mtcars = pd.read.csv('file.csv')
	3. sdf = spark.createDataFrame(mtcars)

Analyzing the data

1. df.printSchema()
2. df.select('column').show(5)

Transformation 

1. retain only relevant data
2. Filters, Joins, Columnar Operations(multiplication, converting from one unit to another), Grouping or Aggregating data 
3. Apply domain specific data augmentations processes 

Loading or Exporting the data

1. final step in ETL pipelines 
2. export to another database
3. export to disk as JSON files 
4. save data to a postgres database 
5. use of an API to export data
