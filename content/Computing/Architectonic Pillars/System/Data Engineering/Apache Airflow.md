---
source: https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Apache%20Airflow/Monitoring%20a%20DAG/Hands-on_Lab-_Monitoring_a_DAG.md.html
---
Open Source workflow orchestration tool

* Build and run workflow 
* workflow is rep as a DAG 
* Data pipeline platform open sourced by Airbnb 
* Only support python 

Challenges 

* Requires own codebase and DevOps 
* Strong coupling of job scheduler with App Code 
* Alternative
	* Nifi 
		* drag and drop 
		* no code 
	* AWS Glue / Azure Data Factory / GCP Dataproc 
	* Kettle by Pentaho 
	* Databricks Jobs / Notebooks 

DAG

1. ![[Computer Science/Data Engineering/_resources/Apache_Airflow.resources/unknown_filename.7.png]]
2. Data in graph stored in form of nodes and connections
3. Acyclic graph avoid cycles
4. Directed as flow has defined direction
5. DAG refers to graph that flows in directed directions and contains no cycles
6. E.g. of DAG is delivery of package to customer (However, your delivery agent in not DAG as he returns back of warehouse after delivery of your products  — hence cyclical)
7. Idempotency (same result, every time) is challenging in data pipeline as there is so many variables
	1. Connectivity interruption
	2. Data quality
	3. Data volatility
	4. Late arrival of data
	5. Business processes
8. DAG developer should check for data incoming and data outgoing to ensure integrity of data pipeline
9. DAG is alternative to blockchain(transparent, immutable, decentralised, unidirectional). It is unidirectional and immutable like blockchain. Blockchain is closely related to list while DAG to graphs
10. Basically, its a block less distributed ledger which is scalable .

Features

* Author, schedule and monitor workflows programmatically. 

4 principles of airflow pipeline

1. Scalable 
2. Dynamic 
3. Extensible 
4. Lean 

![[Computer Science/Data Engineering/_resources/Apache_Airflow.resources/unknown_filename.png]]

![[Computer Science/Data Engineering/_resources/Apache_Airflow.resources/unknown_filename.1.png]]

![[Computer Science/Data Engineering/_resources/Apache_Airflow.resources/unknown_filename.2.png]]

![[Computer Science/Data Engineering/_resources/Apache_Airflow.resources/unknown_filename.3.png]]

Key adv of AA

1. It is rep as DAG
2. expressed as code, making data pipeline more CTM(Collaborative, testable and maintainable) 
3. AA built in operators are used to create tasks (the nodes in a DAG) 

AA Logs 

1. are saved in local file system  
2. can be sent to 
	1. cloud storage 
	2. search engines 
	3. log analyzers 

* * *

* * *

![[Computer Science/Data Engineering/_resources/Apache_Airflow.resources/unknown_filename.4.png]]

![[Computer Science/Data Engineering/_resources/Apache_Airflow.resources/unknown_filename.5.png]]

* * *

* * *

1. Start Apache Airflow.
2. Open the Airflow UI in a browser.
3. List all the DAGs.
4. List the tasks in a DAG.
5. Explore a DAG in the UI.

![[Getting started with Apache Airflow.md.pdf]]

![[Computer Science/Data Engineering/_resources/Apache_Airflow.resources/unknown_filename.6.png]]

Recent Tasks

1. None 
2. Removed
3. Scheduled 
4. Queued 
5. Running 
6. Success 
7. Shutdown 
8. Restarting 
9. Failed 
10. Up\_for\_retry
11. up\_for\_rescheduled 
12. upstream\_failed 
13. skipped 
14. sensing 
15. deferred 

**Actions**
![[image (41).png]]

1. Trigger Dag 
2. Trigger Dag w Configurations ![[image (42).png]]

* * *

* * *

* Search for a DAG.
* Pause/Unpause a DAG.
* Get the Details of a DAG.
* Explore tree view of a DAG.
* Explore graph view of a DAG.
* Explore Calendar view of a DAG.
* Explore Task Duration view of a DAG.
* Explore Details view of a DAG.
* View the source code of a DAG.
* Delete a DAG.

![[Hands-on_Lab-_Monitoring_a_DAG.md.pdf]]

* * *

Interview Question 

1. backfill 
2. catchup 
3. error handling 
4. number of threads 
5. key component of AA architecture -- scheduler is heart and brain 
6. explain external task sensor 
7. How to monitor Airflow 
8. Dynamic Scheduling 
9. Customizing AIRFLOW UI 

key component of AA architecture -

* \- scheduler is heart and brain 
* scheduler and web server resides on the same server 
* scheduler have diff kinds of executors like 
	* local 
	*  salary
	* kubernetes 
* officially supports two dockers 
	* redis -- chache mechanism 
	* levitmq 
* flower 
	* monitor workers and their state 

External task sensor 

1. Can use external task sensor in case of dependency where first DAG should be succesful before running the second DAG. 
2. Parameters provided are 
	1. DAG ID 
	2. task which you want to monitor 

monitoring Airflow 

1. can be fetched by invoking API -- <http://locahost:8080/health>  -> WEBSERVER   (can write a script to schedule a particular url every 5 minutes ) 
2. Output 
	1. "metadatabase" : {
	2.      "status" : "healthy" 
	3. }, 
	4. "scheduler" : {
	5.    "status" : "healthy" 
	6.    "latest\_scheduler\_heartbeat" : "2022-03-24 T 10:19:00 " 
	7. 
	8. }

Dynamic Scheduling 

1. using variable 
2. from airflow.models import Variable 
3. schedule\_interval=Variable.get("pipeline\_schedule") #"

Customizing AIRFLOW UI 

1. can be done by writing plugins 
2. Webserver UI uses FLASK Framework and GUNICORN Server 

* * *

1. Apache Airflow pipelines are built on four main principles. Which of the following principles include parameterization?
2. Which of the following Apache Airflow use cases involves coordination of data in data warehouses?
3. Apache Airflow DAGs are a python script consisting of logical blocks. Which of the following logical blocks might use the ‘from airflow import DAG’ command?
4. Sensors are a class of DAG operators. Which is another type of operator that defines DAG tasks?
5. Which of the following advantages of Apache Airflow expressing workflows as code enables Git to track them?
6. The 'Task Instance Context Menu' can be accessed from any of the DAG views that display what?
7. The final block in your Airflow pipeline script is where you specify the dependencies for your workflow. How do you specify the order of task 1 and task 2?
8. Which block specifies the DAG start date?
9. Which of the following Airflow metrics could fluctuate?
10. Which of the following Apache Airflow basic components serves the interactive UI?
