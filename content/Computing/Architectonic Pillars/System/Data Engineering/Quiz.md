---
source: https://www.coursera.org/learn/etl-and-data-pipelines-shell-airflow-kafka/quiz/qHa3I/practice-quiz-an-introduction-to-data-pipelines/attempt?redirectToCover=true
---
Graded Quiz: ETL and ELT Processes
Latest Submission Grade 100%
1.
Question 1
ETL process consists of Extract > Transform > Load. Which of these three processes is also known as data wrangling? 

1 / 1 point

Extraction 

Data wrangling is a term for another data warehouse process 

Transform 

Load 

Correct
Correct, this process wrangles the data into the format suitable for destination and use.  

2.
Question 2
The ELT process has no information loss. What is the main reason for this benefit?

1 / 1 point

Separation between moving and processing data

Separates the data pipeline from processing

Data source integration

Data replication

Correct
Correct, ELT provides a replica of the source data, and with that no information loss occurs.

3.
Question 3
ETL processes include a storage facility called a staging area. In ELT the staging area fits the description of what?

1 / 1 point

Data warehouse

Data mart

Electronic repository

Data lake

Correct
Correct, the staging area fits the description of a data lake, which is a modern self-serve repository for storing and manipulating raw data.

4.
Question 4
Which of the following pain points does ELT address?

1 / 1 point

Request for fixed processes

Challenges imposed by Big Data

Lack of secure data

Cost effectiveness

Correct
Correct, challenges like scalability imposed by Big Data are addressed.

5.
Question 5
There are many techniques for extracting data. Choosing the technique usually depends on what?

1 / 1 point

Optical or analog

Operating system

Type of client

Intended use

Correct
Correct, it depends on the intended use of the data.

6.
Question 6
Extracting data from IoT devices involves large volumes of redundant data. What is used to decrease the data volume of redundant data and only extract features of interest from raw data?

1 / 1 point

Biometric sensors

APIs

SQL languages

Edge computing

Correct
Correct, edge computing reduces the data volumes of redundant data by extracting features of interest from the raw data.

7.
Question 7
ETL uses the schema-on-write approach and ELT uses the schema-on-read approach. What is the biggest difference in these two approaches?

1 / 1 point

Consistency

Stability

Limited versatility vs. versatility

More data access

Correct
Correct, the ETL approach limits the versatility, while the ELT approach is versatile since it obtains mulitple views of the same source data with ad-hoc schemas.

8.
Question 8
Which of the following examples of information loss during transformation can involve false negatives?

1 / 1 point

Lossy data compression

Edge computing

Aggregation

Filtering

Correct
Correct, Edge computing devices, for example, false negatives appear in surveillance devices designed to only stream alarm signals, not the raw data.

9.
Question 9
Which of the following loading techniques is between batch and stream loading?

1 / 1 point

Micro-batch loading

Parallel loading

On-demand loading

Incremental loading

Correct
Correct, in between batch and stream loading, there is micro-batch loading.

10.
Question 10
Which of the following loading techniques can split a single file into smaller chunks?

1 / 1 point

Scheduled loading

Batch loading

Parallel loading

Stream loading

Correct
Correct, this technique splits single files into small chunks and loads them simultaneously.

* * *

* * *

What is the main purpose of a data pipeline?

**1** **/** **1** **point**

Storage

Move data from one place or form to another

Loading

Data container

**Correct**

Correct, data pipelines move data from one place or form to another.

### **2****.**

Question 2

Which of the following data pipeline processes keep the pipeline running smoothly?

**1** **/** **1** **point**

Monitoring

Maintenance and Optimization

Scheduling and Triggering

Ingestion

**Correct**

Correct, maintenance and optimization keep the pipeline up and running smoothly.

### **3****.**

Question 3

Which of the following is the main reason to use Lambda architecture instead of Micro-batch, Streaming, or Batch data pipelines?

**1** **/** **1** **point**

Accuracy is critical

Help with load balancing

Records are processed immediately

Access to earlier data and speed is important

**Correct**

Correct, Lambda can be used in cases where access to earlier data is required but speed is also important.

### **4****.**

Question 4

Which of the following data pipeline tools is specific to ELT?

**1** **/** **1** **point**

AWS Glue

Panoply

Alteryx

Talend Open Studio

**Correct**

Correct, Panoply is an ELT-specific platform.

### **5****.**

Question 5

There are always stages that are bottlenecks in the data flow of pipelines. Which of the following is a simple way to load balance pipelines?

**1** **/** **1** **point**

Parallelize

Serialize

Buffers

Queues

**Correct**

Correct, this distributes data packets as they arrive and helps balance loads.

* * *

* * *

How does data flow through pipelines?

**1** **/** **1** **point**

Software processes

Data packets

Files

Processing threads

**Correct**

Correct, data flows through a pipeline in the form of data packets.

### **2****.**

Question 2

Which of the following pipeline monitoring considerations affects the amount of data that passes through the pipeline over time?

**0** **/** **1** **point**

Utilization

Logging and alerting system

Latency

Throughput

**Incorrect**

Incorrect, please review the Key Data Pipeline Processes video.

### **3****.**

Question 3

Which of the following data pipelines corresponds with the fraud detection use case?

**1** **/** **1** **point**

Micro-batch data pipeline

Batch data pipeline

Lambda architectures

Streaming data pipeline

**Correct**

Correct, streaming data pipelines are used for fraud detection.

### **4****.**

Question 4

Which streaming data pipeline tool allows you to build applications using the Streams Processing Language (SPL)?

**1** **/** **1** **point**

IBM Streams

Apache Spark

Apache Samza

SQLstream

**Correct**

Correct, IBM Streams lets you build real-time analytical applications using the Streams Processing Language, or SPL, plus Java, Python, or C++.

### **5****.**

Question 5

Pipelines that incorporate parallelism are referred to as being\_\_\_\_\_ ?

**1** **/** **1** **point**

Linear

Dynamic or non-linear

Static

Aligned

**Correct**

Correct, pipelines that incorporate parallelism are referred to as being dynamic or non-linear.

### **6****.**

Question 6

Batch data pipelines usually run periodically on fixed schedules. Which of the following is another method to run these?

**1** **/** **1** **point**

Manually

Error occurrence

Flags

Triggers

**Correct**

Correct, Batch processes typically operate periodically on a fixed schedule – ranging from hours to weeks apart. They can also be initiated based on triggers, such as when the data accumulating at the source reaches a certain size.

### **7****.**

Question 7

Which of the following common features of modern ETL and ELT products is known as "no-code"?

**1** **/** **1** **point**

Security

Drag-and-drop

Data crawling

Fully automated

**Correct**

Correct, a drag-and-drop GUI for specifying rules and data pipeline flows – also known as “no-code” ETL.

### **8****.**

Question 8

Which of the following data pipeline use cases is the simplest?

**1** **/** **1** **point**

File backup

Transactional record movement

Raw data preparation

Send/receive messages

**Correct**

Correct, the simplest pipeline is one which has no transformations and is used to copy data from one location to another, as in file backups.

### **9****.**

Question 9

Latency is the total time it takes for a single packet of data to pass through the pipeline. Which of the following limits latency?

**0** **/** **1** **point**

Slowest process

Bad data

Small data packets

Data leak

**Incorrect**

Incorrect, please review the Introduction to Data Pipelines video.

### **10****.**

Question 10

Micro-batch data pipelines decrease the batch size. Which of the following do micro-batch pipelines **increase**?

**1** **/** **1** **point**

Storage

Simple transformation

Latency

Batch process refresh rate

**Correct**

Correct, the refresh rate of individual batch processes is increased to achieve near-real-time processing.
