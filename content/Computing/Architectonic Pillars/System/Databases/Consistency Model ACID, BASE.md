---
---
**Consistency Model** 

1. ACID -- used by relational DB -- data consistency 
2. BASE -- used by non relational DB -- data availability 

* * *

**Acid** 

1. Acronym that stands for 
	1. atomicity 
	2. consistency 
	3. isolation 
	4. durability 
2. Atomicity 
	1. Does this query happen all at once ? 
	2. all operations succeed or every operation is rolled back. 
	3. e.g. transaction, debit and credit should be part of one transaction, else amount may be debited without anyone getting any credit. 
3. Consistency 
	1. maintaining structural integrity of data in DB is not compromised 
	2. across servers, considering the scenario if primary database fails 
4. Isolation 
	1. transactions not compromising integrity of other transactions in progress 
	2. that is, elements working concurrently or sequentially 
5. Durability 
	1. Restoring after a server crash 
	2. e.g. if databases runs only on memory 
	3. exist trade offs b/w durability and time taken for running query. If saved on disk, takes alot of time to run the query. 
	4. No data acceptable/unacceptable to lose -- its all about choices 

Use case
1. Ensure consistency of performed transaction 
2. Used by 
	1. Fin Institution -- money transfer 
	2. Data Warehousing 
3. DB can handle many small simultaneous transactions 

* * *

**BASE**

1. BA : Basically Available 
	1. ensures avl of data by storing and replicating it across nodes of DB cluster 
2. S : Soft State 
	1. lack of immediate consistency, data may change over time 
3. E : Eventually consistent 
	1. Not consistent right away, but achieve consistency over a time. 

Use Case
1. NO SQL -- few req for immediate consistency, data freshness, and accuracy 
2. Benefits -- avl, scale and resilience
3. Marketing and customer service coys 
4. social media application 
5. worldwide avl online service -- netflix, uber, spotify  
6. favors avl over consistency 

* * *

Transaction

1. Sets of queries that is carried out for one task. 
2. treated as one, as performed simultaneously. 
3. Query needs to work in same way every time, no matter how you send it -- multiple threads.
