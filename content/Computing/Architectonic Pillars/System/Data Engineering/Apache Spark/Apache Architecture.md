---
---
1. Architecture of Spark
2. Processes in a Spark Application 
3. Spark Jobs and Tasks
4. Stages 
5. Data Shuffle 
6. Cluster Deploy Modes the driver program can be run in 

Spark Architecture has driver and executor processes, coordinated by the Spark Context in the Driver. Driver creates jobs and the Spark Context spits them into tasks which can be run in parallel in the executors on the cluster. 

![[Computing/Architectonic Pillars/System/Data Engineering/Apache Spark/_resources/Apache_Architecture.resources/unknown_filename.4.png]]

Spark : Two Main Processes

1. Driver Prog 
	1. Single process that creates work for the cluster 
	2. Runs as one prog per application 
	3. runs user code, create works and sends to cluster 
2. Executors
	1. process running multiple threads that perform work concurrently 
	2. multiple processes throughout the cluster that carries out work in parallel  
	3. They works independently, 
	4. ![[Computing/Architectonic Pillars/System/Data Engineering/Apache Spark/_resources/Apache_Architecture.resources/unknown_filename.3.png]]

Spark Context 

1. starts when application launches
2. must be created in driver before RDD or DF
3. Communicates with the cluster manager 
4. One SC per Spark Application 
5. ![[Computing/Architectonic Pillars/System/Data Engineering/Apache Spark/_resources/Apache_Architecture.resources/unknown_filename.png]]

Spark Jobs

1. Driver prog creates work from user code called jobs
2. Jobs are computations that can be performed in parallel. 
3. SC divides jobs into tasks to be executed on cluster 

Spark Tasks

1. Given jobs performed on different data subsets called partition, can be executed in parallel in executor.
2. A worker is a cluster node that can launch executor processes to run task 
3. Each executor is allocated local resources in terms of memory and cores 
4. Executor outputs result to RDD or return to Driver 
5. Increasing cores and executors increases parallelism
6. ![[Computing/Architectonic Pillars/System/Data Engineering/Apache Spark/_resources/Apache_Architecture.resources/unknown_filename.1.png]]

Spark Stages and Shuffle 

1. Stages are set of task that are seperated by data shuffle 
2. Stage is set of tasks within a job that can be completed n the current local data partition 
3. Shuffle marks boundary b/w stages, predecessor stage must complete before start of new stage 
4. Stages connects to form a dependency graph 
5. Shuffle are costly because requires 
	1. data serialization 
	2. disk and network I/O
6. Shuffle is necessary when operation requires data outside the current partition of the task 
7. ![[IMG_1949.jpg]]

Driver Deploy Modes  : two types

1. Client Mode -- driver process is launched outside the cluster 
2. Cluster Mode --driver process inside the cluster node 

Executor - Core combination

1. Ideal - 1 x 8 

![[Computing/Architectonic Pillars/System/Data Engineering/Apache Spark/_resources/Apache_Architecture.resources/unknown_filename.2.png]]
