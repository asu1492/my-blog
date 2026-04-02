---
---
What is DP 

* applies to processes that are connected to each other sequentially, that is, output of one process is passed along as input to the next process in a chain 
* Purpose f DP is to move data from one place, or form, to another 
* System which extracts data and passes it along to optional transformation stages for final loading.  (ETL/ELT)
* Includes
	* low level hardware architectures 
	* software driven processes -- commands, programs, and threads
* Bash pipe command in linux which can be used as a the 'glue' that connects such processes together. 
* Data flow through DP in the form of packets(can be a single record or a large collection of data). Packets are queued for injestion to the pipeline 
* Packet -- broadly refers to units of data 

DP performance 
![[Computer Science/Data Engineering/_resources/Data_Pipelines.resources/unknown_filename.png]]

* Latency and throughput are key design considerations for data pipelines. 
* Latency 
	* total time it takes for a single packet of data to pass through the pipeline 
	* Also, latency can be defined as sum of the individual times spent during each processing stage within the pipeline. 
	* Overall latency is limited by the slowest process in the pipeline. 
	* ![[Computer Science/Data Engineering/_resources/Data_Pipelines.resources/unknown_filename.1.png]]
* Throughput 
	* refers to how much data can be fed through the pipeline per unit of time. 
	* processing large packets per unit time increases throughput 
	* T Delays means time b/w successive packet arrivals. 
	

Use Cases

* Backup systems
* integrating disparate raw data sources into a data lake 
* moving transactional record to data warehouse 
* Streaming data from IOT devices to make info avl in the form of dashboards or alerting systems. 
* preparing raw data for ML developments or production. 
* Message sending/receiving -- sms, video meeting, email, etc. 

DP processes 

* Typical stages in DP processes
	* **Extraction** of data from one or more data sources
	* **Injestion** of extracted data into the pipeline 
	* Optional **data transformation** stages within the pipeline 
	* Loading of data into a **destination facility** 
* mechanism for scheduling or triggering jobs to run 
	
* Monitoring, Maintenance and Optimization 
	* Monitoring of DP once it is production to ensure data integrity 
	* M & O for smooth functioning/running 
		

Key Monitoring considerations in DP 

* Latency
* Throughput demand -- volume f data passing through the pipeline over time 
* Errors and Failures -- caused by factors such as network overloading, failures at source or destination systems. 
* Utilization rate -- how fully the pipeline resources are used, affects cost. 
* Should have system for logging events and alerting admin when failures occur. 

DP Solutions for mitigating data flow bottlenecks 

* Load balance DP 
	* no upstream dataflow bottlenecks 
	* JIT avl of data packets
	* Uniform packet throughput for each stage 
	* all stages takes same amt of time to process a packet 
* DP are rarely load balanced due to time and cost considerations 
* If some stage has bottlnecks 
	* should be parallelized by splitting the data, reducing latency
	* ther is little overhead in managing parallelization and recombining output back into the pipeline, but the overall latency wl be reduced. 
* Bottlenecks can be mitigated by parallelisation and I/O buffers
* Parallelization 
	* parallelize a process by replicating it on multiple CPUs, cores or threasds 
	* Pipelines with // are refered as dynamic or non linear, as opposed to 'static', which applies to serial pipelines. 
* Stage synchronisation / I/O Buffers
	* I/O buffers can help synchronise stages
	* I/O buffer is a holding area for data, placed b/w processing stages having different or varying delays.
	* Buffers can also be used to regulate the output of stages having different or variable processing rates, thus can be used to improve throughput.

Two main paradigms within data pipeline engineering
![[Computer Science/Data Engineering/_resources/Data_Pipelines.resources/unknown_filename.2.png]]

1. Use case for Batch, streaming DP comes down to a trade off b/w
	1. Accuracy
	2. Latency requirements
2. If we need lower latency, then tolerance for fault has to increase
3. Batch DP
	1. Are used when datasets need to be extracted and operated on as one big unit.
	2. Operates periodically on fixed schedule — hr/days or can be based on trigger, that is, data reaches a threshold
	3. Used when accuracy is critical and appropriate for cases that don’t depend upon recency of data.
4. Streaming DP
	1. Used to inject packets of info, as they arrive, one by one in rapid succession. E.g. financial transaction, social media interaction
	2. Used where results are req with min latency, essentially in real time.
	3. Achieved by decreasing batch size and increasing refresh rate of individual batch processes.
	4. Use cases : streaming of movies, music, social media feeds and sentiment analysis,  user behav analysis and targeted ads, real time product pricing and recommender systems.

Micro Batch

1. Help in load balancing and lower latency
2. Can be used to simulate real time data streaming

Hybrid Lambda DP

1. Accuracy and speed
2. Combines bath and streaming DP methods such.
3. Historical data delivered in batches to batch layer
4. Real-time data is streamed to a speed layer. This is used to fill ‘latency gap’ caused by batch processing.
5. Used where access to earlier data is req but speed is equally imp.
6. Challenge : complex design
7. Use case : periodic data backups, weather forecasting.

Features of Modern DP tools

1. Fully automated pipeline
2. Ease f USe
3. Drag and Drop interface : no code ETL  and data flows
4. Transformation support
5. Security and compliance with HTPAA and GDPR

Open source DP tools

1. The Pandas Python Library
	1. Versatile and popular programming tool
	2. Uses data structure called a data frames to handle Excel, CSV style tabular data
	3. Great tool for ETL, data exploration and prototyping
	4. Since DF manipulations are carried in-memory, doesn’t readily scale to Big Data
2. Lib with similar APIs are
	1. Vaex
	2. Dask
	3. Spark
3. Scalable DF APIs such as PostgreSQL
4. Apace Airflow
	1. Another package based on the Python
	2. Open source configuration as code DP platform.

Various Enterprise Data Pipeline tools  / Batch Processing

* Apache Airflow 
* Talend - drag and drop GUI that automatically generates Java Code
* AWS Glue — ETL service that simplifies data prep for analytics and suggest schema for storing data
* Panoply : ELT pipeline — no code, SQL based view creation, shift emphasis from Data development to data analytics. Integrates with Tableau and powerBI 
* Alteryx : both ELT and ETL
* IBM InfoSphere DataStage : ELT and ETL jobs — drag and drop
* IBM Stream — build real time analytics application using SPL, Java, Python, C++; combines data in rest and motion for deliver intelligence in real-time
	

Streaming Data Pipeline tools 

* Apache Storm 
* Apache Samza 
* Apache Spark 
* SQL Stream 
* Apache Kafka
* Azure Stream
