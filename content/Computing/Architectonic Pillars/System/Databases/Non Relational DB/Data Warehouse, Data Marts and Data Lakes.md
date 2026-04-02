---
source: https://www.coursera.org/learn/getting-started-with-data-warehousing-and-bi-analytics/supplement/I2886/data-warehousing-with-star-and-snowflake-schemas
---
Data Stores -- 3 Operational DS are
1. Enterprise DWS
2. Data Lakes
3. Data Marts

DW
1. large repository of data, cleaned for consistency and quality and have single source of truth (current and historical data)
2. contains
	1. facts and fact tables
	2. dimensions and dimension tables
	3. retrieval of aggregate data - CUBE and ROLLUP
	4. Schema : star and snowflakes
3. aggregate data from one or more source into a single consistent data store to support data analytics.

Hosting of DW systems
1. Onsite / On premise within enterprise data centres
2. On Appliance
	1. Since 2000s
	2. Specialized hardware with pre integrated software
3. Cloud
	1. Since 2010's
	2. pay as you go service
	3. eliminate overhead hardware cost
	4. CDWs (Cloud Data Warehouses )

Cost involved in DWS
1. Infra
2. Compute and Storage
3. Migration of Data
4. Admin
5. Data Maintenance

DWS are used for
1. Data mining -- AI and ML
2. Data transformation during ETL process
	1. frontend reporting
	2. OLAP Processing - fast, flexible and multidimensional data analysis

+ve f DWS

1. Centralises data from disparate system - transactional system, flat files, operational databases
2. data integration by removing bad data -- single source of truth
3. smarter decision making using BI

E.g. f DWS

1. Oracle Exadata
	1. on premise and cloud
	2. all types of workloads - OLTP, In memory analysis, DW analysis, Mixed
2. IBM Netezza
3. Amazon Redshift
	1. graph optimize algo, automatically organise and store data.
4. Snowflakes
	1. Always on encryption
	2. GDPR and CCPA compliant
5. Google Bigquery
6. Azure Synapse
7. Teradata Vantage
8. Oracle Autonomous DW
9. Vertica
10. IBM Db2 Warehouse
	1. BLU acceleration -- in memory SQL columnar processing, data skipping and MPP(Massive Parallel Processing)

* * *

Data Marts

1. DL are isolated part f large enterprise DW that are specifically built to serve particular business function, purpose and community f users.
2. known for specific, timely and rapid support for decision making.
3. RDBs with star or snowflake schema
4. central fact table f business metric, surrounded by associates dimension tables.

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.10.png]]

Types f DM based on their rel with DW

1. Independent - created directly from source
2. Dependent - draw data from data warehouse
3. Hybrid

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.6.png]]

Purpose f DM
![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.9.png]]
![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.2.png]]

* * *

  Data Lakes

1. are self serve staging area for transforming data, prior to loading in DM and DW  
2. DL are repository f raw data, straight from the source.
3. It is a storage repository that stores large amount f structured, semi structured and unstructured data in native format classified and tagged with metadata.
4. reference architecture independent f tech, w/o defining structure or schema  
5. deployed using
	1. S3
	2. NoSQL
	3. RDBS
	4. Apache Hadoop

DM vs DW
![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.8.png]]

Benefits f DL
![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.3.png]]

**Data Lakes vs Data Warehouse**

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.5.png]]![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.1.png]]
![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.4.png]]
![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.7.png]]

* * *

* * *

**Enterprise DW Architecture**
![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.12.png]]

1. Hub and Spoke Model, when multiple data marts are involved.

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.11.png]]

![[Overview of Data Warehouse Architectures _ Courser.1.png]]![[Overview of Data Warehouse Architectures _ Courser.png]]

* * *

* * *

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.13.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.18.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.19.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.22.png]]
Why Normalization

1. Reduces redundancy
2. reduces data size

Normalizing a table means to create, for each dimension:
1\. A surrogate key to replace the natural key, that is, the unique values of the given column, and
2\. A lookup table to store the surrogate and natural key pairs.

* * *

* * *

Staging Areas
![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.21.png]]
![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.17.png]]

* * *

* * *

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.20.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.16.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.15.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.14.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.27.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.28.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.33.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.29.png]]
![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.31.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.32.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.34.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.24.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.30.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.25.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.23.png]]

![[Computer Science/Databases/_resources/Data_Warehouse,_Data_Marts_and_Data_Lakes.resources/unknown_filename.26.png]]

* * *

What is Analytics 

* building models for better decision making 
* is methodical compilation and dissection of 
	* data
	* statistics 
	* operational research 
		

Tools that have revolutionised Analytics 

* Storage - large vol of data with low latency 
* Processes - faster P, resulting in faster turnaround time 
* Memory 

Analytical Outcomes 

* Descriptive - what happened in the past 
* Diagnostic
* Prescriptive - what can happen in the future 
* Predictive - what should happen in the future 

Why BI tools 

* Enables data preparation 
* mining 
* data mgmt 
* data visualization 
* focus on 'what' and 'why' of businesses 
* transform data into opportunity 

**Various BI Tools** 
![[photo_2022-04-20_14-33-43.jpg]]

---

## Star Schema vs Snowflake Schema

| Attribute | Star schema | Snowflake schema |
| --- | --- | --- |
| **Read speed** | Fast | Moderate |
| **Write speed** | Moderate | Fast |
| **Storage space** | Moderate to high | Low to moderate |
| **Data integrity risk** | Low to moderate | Low |
| **Query complexity** | Simple to moderate | Moderate to complex |
| **Schema complexity** | Simple to moderate | Moderate to complex |
| **Dimension hierarchies** | Denormalized single tables | Normalized over multiple tables |
| **Joins per dimension hierarchy** | One | One per level |
| **Ideal use** | OLAP systems, Data Marts | OLTP systems |

**Key rule of thumb:**
- Star schemas are optimized for **reads** (OLAP, data marts). Dimensions are denormalized/flattened.
- Snowflake schemas are optimized for **writes** (transactional DW). Dimensions are fully normalized.
- A star schema is a special case of a snowflake schema where all hierarchies have been collapsed.
- ETL pipelines moving OLTP → OLAP include a **denormalization step** that collapses snowflake hierarchies into star dimensions.
- You can also layer materialized views of a star schema *on top of* a snowflake schema to get the best of both worlds.
