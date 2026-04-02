| References 
1. https://app.excalidraw.com/l/56zGeHiLyKZ/AjAIb7q1lP8

###### What is data Model 
1. DM defines how your data is 
	1. Structured
	2. Stored 
	3. Related 
2. ![[Pasted image 20260302162756.png]]


###### DB Model Optional 
1. [[Relational Databases]]  
2. Document DB
	1. When ? Schema Flexibility 
	2. [[MongoDB]]
	3. FireStone 
3. KV Store 
	1. Redis
	2. DynamoDB 
4. Wide Column DB
	1. Makes Write faster 
	2. Time-Series, Events
5. Graph DB
	1. Nodes and Edges 


##### Schema Design 
1. Three Key Factors
	1. Data Volume
		1. will it fit on one node or do you need sharding/partitioning ?
	2. Access Patterns 
		1. How is data queried
		2. derived from APIs; they dictate normalization, indexes, and sometimes denormalization.​
	3. Consistency Requirement 
		1. How Strict ?
		2. ACID vs eventual consistency([[Consistency Model ACID, BASE]])
2. PK - unique identifier for each record which is monotonically increasing Id 
3. FK - points to primary key in another table to create relationship - enforce Referential Integrity, e.g. cannot let you create a post if userId is null. 
4. ![[Pasted image 20260302170143.png]]

How to make retrieval faster
1. Indexing : 
	1. Index columns that support your most common queries (e.g., index Posts.user_id for “get posts by user”, index Comments.post_id for “get comments for post”), and derive these directly from each API endpoint.
2. Denormalization - 
	1. e.g. User and Post two normalized tables, without duplication. Only denormalize for clear, articulated performance reasons that indexing cannot solve.
	2. Prefer denormalization in caches (e.g., precomputed feeds) where some eventual consistency is acceptable, keeping the primary DB normalized.​
3. [[Sharding]]
	1. Why 
		1. Only shard after you estimate that a single node cannot hold the data; 
	2. How 
		1. Choose a shard key that aligns with primary access patterns to avoid cross‑shard joins.
		2. Keep strongly related entities on the same shard (e.g., shard by post_id and ensure comments for that post live on the same shard) to avoid expensive multi-shard queries.
4. ![[Pasted image 20260302172142.png]]

##### Practical “recipe” to follow in interviews
When you draw the DB box in your system design answer, systematically do:
1. Choose DB type (usually relational/Postgres) and justify briefly.​
2. List necessary columns for each table, just enough to satisfy functional requirements.​
3. Mark primary keys and foreign keys and mention key constraints (e.g., UNIQUE, NOT NULL).
4. Don't get bogged down by many-to-one / one to many, just define your FK and let relationship speaks for themselves. ​
5. Add indexes driven from each API’s query patterns.​
6. Decide if any denormalization is needed (often in cache only) and state why.​
7. If data size demands it, specify sharding key and reasoning (keep related data colocated, avoid cross-shard joins).
