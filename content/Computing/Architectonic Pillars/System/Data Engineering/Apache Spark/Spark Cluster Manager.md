---
---
1. Modes of running Spark on a Cluster 
2. Components and benefits of each cluster mode
3. How to run spark to connect with each cluster moe
4. How and when to set up a local spark instance 

Cluster managers acquire resources and run as a an abstracted service outside the application 

Spark Cluster Manager 

1. It communicates with a particular cluster to acquire resources for running application 
2. Runs as a services outside the application and abstracts the cluster type 
3. SC creates tasks and communicate to cluster manager what resources are needed
4. Cluster manager reserves executor core and memory resources 

Types of Cluster Manager 

1. Spark Standalone - comes with Spark 
2. Apache Hadoop YARN - Yet another resource Manager from Hadoop project
3. Apache Mesos - General purpose manager with additional benefits 
4. Kubernetes - open source sys for running containerized applications 

Selection of right cluster manager depends on

1. data ecoystem 
2. ease of cofiguration 
3. portability 
4. deployment 
5. data partitioning needs 

Components on Spark Standalone 

1. Workers - run an executor process to receive tasks. 
2. Master - connects and adds workers to the cluster 

Setting up of Spark Standalone

1. Starting the master -- ./sbin/start-master.sh  (Note: Master is assigned URL and PORT Number ) 
2. starting worker with master URL -- ./sbin/start-slave.sh  spark://<master-spark-URL>:7077 (Note: Enabled bidirectional communication b/w masters and workers) 
3. Launch Spark Application on clusters by specifying the Master URL 
	1. ./bin/spark-submit \\
	2. \--master spark://<spark-master-URL>:7077 \\
	3. <additional configuration> 

Running Apache YARN

1. ./bin/spark-submit \\
2. \--master YARN \\
3. <additional configuration>

Apache Mesos 

1. Benefits includes, making partition
	1. scalable b/w many spark instances
	2. dynamic b/w Spark and other big data frameworks 
2. Configuration : [spark.apache.org/docs/latest/runnin](http://spark.apache.org/docs/latest/running)g-on-mesos.html

Spark Kubernetes

1. Run containerized applications, making it easier to 
	1. automate deployment 
	2. simplify dependency management 
	3. scale the cluster 
2. Launching Kubernetes on spark 
	1. ./bin/spark-submit \\ 
	2. \--master k8s://https://<k8s-apiserver-host>;<k8s-apiserver-port> \\ 
	3. <additional configuration >

Local Mode 

1. Does not connect to a cluster 
2. Run in the same process that calls 'spark-submit' and uses threads for running executor tasks 
3. not designed for optimal process
4. used for testing or debugging purposes 
5. Setup 
	1. \## Launch Spark in local mode with 8 cores
	2. ./bin/spark-submit \\
	3. \--master local\[8\] \\
	4. <additional configuration> 
6. Use of \* to specify using all avl cores 
	1. ./bin/spark-submit \\
	2. \--master local\[\*\] \\
	3. <additional configuration> 

* * *

Note:

Containerized application 

1. Container tech genesis in Linux kernel C Group 

1. Background :
	1. Use of VM that ran all sorts of prog like APP, SSH, and managed through bash scripts/Chef/Puppet --> did not know what failed and how to fix it -- then came, VM Image, which was a boon for architectures but bane for developers -- to ease things on developer side, we came up with container image 
	2. Container image separates b/w 
		1. APP 
		2. Kernel 
2. Enable developer to push their application to container repository (Azure Container Repository) and they can pull this image wherever they wants -- data centre, cloud, around the world.
3. Also enable, repackaging of legacy software and better resource utilization by running multiple container image of the same software 
4. Container do not provide security boundary. These boundaries are more like resource partitioning -- memory, CPU usage .. For security, may require deployment of hypervisor 
5. Container Orchestrator -- Kubernetes enable distribution of container images 

|     |     |
| --- | --- |
| VM<br><br>1. Hypervisor for spinning up VMs <br>2. Running application and scaling it require deployment of VM. VMs are resource heavy. Smallest Node.js VM is 400 MB <br>3. Challenges in Agile DevOPS as may work well in local m/c but starts to break as we push in production | Containers <br><br>1. Containerization is 3 Step Process<br>	1. Manifest : description <br>	2. Create Actual Image <br>	3. Actual container with its binaries and libraries required to run the prog |

![[Computing/Architectonic Pillars/System/Data Engineering/Apache Spark/_resources/Spark_Cluster_Manager.resources/unknown_filename.png]]

![[Computing/Architectonic Pillars/System/Data Engineering/Apache Spark/_resources/Spark_Cluster_Manager.resources/unknown_filename.1.png]]
