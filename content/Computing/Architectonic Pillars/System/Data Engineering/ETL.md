---
source: https://www.coursera.org/learn/etl-and-data-pipelines-shell-airflow-kafka/supplement/tyIMH/etl-techniques
---
![[Computer Science/Data Engineering/_resources/ETL.resources/unknown_filename.7.png]]

![[Computer Science/Data Engineering/_resources/ETL.resources/Image.png]]
Following techniques are used to bring your data into your cloud datastore / data pipeline deployment

1. ETL 
2. ELT
3. bulk ingestion 

ETL

1. ETL is essential process in any data pipeline. It is first step, makes data accessible to data warehouses for downstream application.
2. It is an acronym for an automated data pipeline engineering methodology
3. ETL Process
	1. Curating data from multiple sources and conforming it to an unified data format or structure. Finally loading the transformed data into its new env.
4. Data Extraction
	1. Process that reads or obtains data from one or more sources 
	2. Extraction requires to configure access to data and read it into an application
	3. E.g. we scraping — data is extracted from web pages using python, R to parse underlying HTML; using API to programmatically connect to data and query it
	4. In case of static data, extraction would be stage in batch process
	5. DE from disparate sources , such as,
		1. Sensor data from IoT devices
		2. Unstructured data from scanned medical documents, or company emails
		3. Streaming data from social media network, real time stock market transaction,,
		4. Data from existing enterprise databases and data warehouses
5. Data Transformation
	1. Done in intermediate working env known as staging area.
	2. Transformation process wrangles data into format that is suitable for its destination and intended use
	3. Also known as data wrangling
	4. Processing data to make it conform to the requirement of both the target sys and the intended use case for the curated data
	5. Includes
		1. Cleaning - to ensure data realisability and conform with the target syste,
		2. Filtering - selecting what is needed
		3. Joining- merging disparate sources
		4. Normalising - converting data to common units
		5. Data Structuring — converting data from one data format to another
		6. Feature engineering : KPI for dashboard, ML
		7. Anonymising and Encrypting - ensuring privacy and security
		8. Aggregating — summarzing data
		9. Formatting and data typing — making data compatible with its destination 
6. Data Loading
	1. Loading process takes the transformed data and loads it into its new env.
	2. Thus data is now ready for visualisation, exploration and further transformation
	3.   New Env — eg. are database, data warehouse, data mart
7.  Use Cases
	1. Digitising analog media — paper photos, docs,  analogy video and audio ; digital transformation  & storing it in db.
	2. Moving data from OLTP to OLAP
		1. OLTP — online transaction processing system don’t save historical data
		2. OLAP — online analytical processing system
	3. Injection by dashboards use by operations, sales, marketing, etc.
	4. Training and deploying ML Models for prediction and augmented decision making
8. 3 ETL Challenges + 2  -- these challenges are solved by ELT process 
	1. Scaling 
	2. Transforming data accurately 
	3. Handling diverse data sources 
	4. Siloed info
	5. lengthy time to insight 

Traditionally, the overall accuracy of the ETL workflow has been a more important requirement than speed, although efficiency is usually an important factor in minimizing resource costs. To boost efficiency, data is fed through a data pipeline in smaller packets (see Figure 2). While one packet is being extracted, an earlier packet is being transformed, and another is being loaded. In this way, data can keep moving through the workflow without interruption. Any remaining bottlenecks within the pipeline can often be handled by parallelizing slower tasks.

![[Computer Science/Data Engineering/_resources/ETL.resources/unknown_filename.6.png]]

* * *

* * *

ELT

1. ELT Process
	1. acronym for a specific automated data pipeline engineering methodology
	2. known for FSS - flexibility, scalability and speed 
	3. similar stages as ETL, but  order in which they are performed is different
	4. Extraction process obtains the data from all sources and reads the data, often in an asynchronous fashion, into an application
	5. Often such application are data lake
	6. clean separation between moving data and processing data
	7. On demand transformation 
	8. Enable self serving, ad hoc analytics in real time 
2. In terms of speed, moving data is usually more of a bottleneck than processing it, so the less you move it, the better
3. ELT may be your best bet when you want flexibility in building a suite of data products from the same sources. Also, since we are working on replicas of source data, there is no info loss (Often transformation results in data losses)
4. Use Cases
	1. Cloud Computing -- scalability, pay as you use model thus save $ on resources, also no worry over under utilization of resources and overspending.  
	2. Streaming analytics 
	3. integration of highly distributed data sources from across the globe 
	4. Multiple data products from the same sources
5. Why its an emerging trend 
	1. largely due to cloud platform technologies
	2. more flexibility, no info loss 

ETL vs ELT 

|     |     |
| --- | --- |
| Rigid - pipelines are engineered to user specifications | Flexible - end users build their own transformations |
| largely used for relational data, on premise, scalability is difficult | Make use of on demand scalability of cloud and handles both structured and unstructured data |
| Workflows development takes time | Real time in interactive env |

![[Computer Science/Data Engineering/_resources/ETL.resources/unknown_filename.5.png]]

1. ELT emerge as result of bigdata processing 
2. In ELT, all data resides in data lake(raw data, purpose of data is not defined) 
3. Loading of data takes place in raw format reserving transformation for people to apply themselves in a ‘self serving analytics’ destination env.
4. In Data Lake, each project performs data transformation, as and when required. While in ETL, all necessary transformation usage scenario is pre defined. 
5. Transformations in ELT are decoupld from the data pipelines 

ETL using shell scripting

![[Computer Science/Data Engineering/_resources/ETL.resources/unknown_filename.1.png]]

![[Computer Science/Data Engineering/_resources/ETL.resources/unknown_filename.4.png]]

![[Computer Science/Data Engineering/_resources/ETL.resources/unknown_filename.jpeg]]

Cloud Based ELT Operations 
![[Computer Science/Data Engineering/_resources/ETL.resources/unknown_filename.3.png]]

![[Computer Science/Data Engineering/_resources/ETL.resources/unknown_filename.2.png]]

* * *

* * *

Schema
What is ETL
Use of shell scripting for implementing an ETL Pipeline
Scheduling ETL scripts/task using cron

\*\*\*\*\*\*\*\*\*\*\*\*
What is ETL
\*\*\*\*\*\*\*\*\*\*\*\*
ETL refers to extract, transform and load
    extract data from siloed system
    transform it into destination format
ETL has roots in the 1970s with rise of centralized data repositories
ETL software proliferated with data warehouses in 1990s for data storage and transformation. However  
these operations were carried on-premise data warehouses  
Cloud brought scalability and cost effectiveness. It also moves beyond ETL to ELT, that is, it
extract and load data and then transform.
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Why ETL
\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Context       -- deep historical context with data
Consolidation -- consolidated view of data for easier analysis and reporing
Productivity  --
Accuracy      -- data accuracy and audit compliance

\*\*\*\*\*\*\*\*\*\*\*\*
Application of ETL
Traditional Uses
    - Combine structured and unstructured data and land them in data warehouse -- optimized through manipulating into table structure for insights  and visualization
    - Migrate from legacy warehouse to the cloud.
Big Data
    - flowing data from IOT, Social Media, video, log mining -- need f this broadscope data to      gain competitive edge
Hadoop
