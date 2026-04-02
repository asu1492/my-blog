
###### What is caching 
1. A cache is **temporary** storage that keeps recently or frequently used data in a faster layer (often RAM) to avoid repeatedly hitting a slower source like a disk-backed database
2. Typical numbers: disk access is around 1 ms vs memory ~100 ns, so caching can be ~10,000x faster and critical at high QPS.
###### Where to place a cache
1. External Caching 
	1. Application Server first checks the cache, if misses then read from DB as a fallback.
	2. EC examples are Redis, Memcached
	3. Tradeoff: Expensive network hop 
2. In‑process cache
	1. data stored in the **app’s own memory,** fastest but not shared across instances and can lead to duplication/inconsistency
	2. use for ultra‑low latency config/lookup data
3. CDN: 
	1. caches content geographically close to users, primarily **optimizing network latency**; best to mention for static media and global users.
	2. Generally used for 
		1. Media Delivery
		2. Static Files 
4. Client‑side caching: browser/app local storage or in‑app memory; useful for offline or client‑heavy workloads, but you have less control and more staleness risk

###### Caching Architectures 
- Cache‑aside (default): app reads cache first; on miss, reads DB, writes to cache, returns result. Keeps cache lean and is simplest to explain and implement.​ ![[Pasted image 20260302191242.png]]
- Write‑through: write goes to cache, then synchronously to DB before completion; gives fresher reads but slower writes and dual‑write complexity(when cache write succeed but DB fails, need error handling, retry-mechanism and so on).​ ![[Pasted image 20260302190934.png]]
- Write‑behind (write‑back): write only to cache; cache flushes to DB asynchronously in batches; high write throughput but risk of data loss if cache fails.​![[Pasted image 20260302191117.png]]
- Read‑through: app always talks to cache; on miss, cache itself fetches from DB and populates; similar to cache‑aside but cache acts as proxy, typical in CDNs.​![[Pasted image 20260302191150.png]]
- In interviews, you usually stick to cache‑aside unless you have a very clear justification for others.

###### Cache eviction policies 
- LRU (least recently used): evict item that hasn’t been accessed for the longest time; common default in interviews.​
- LFU (least frequently used): evict least often accessed items; useful when access patterns are highly skewed.​
- FIFO: evict oldest inserted item; conceptually simple but rarely the right choice.​
- TTL: each entry has an expiration time; great when freshness is more important than keeping everything in memory (sessions, feeds, API responses).
###### Common Issues that come up from caching 
- **Cache stampede / thundering herd:** 
	- many requests try to rebuild an expired popular key at once, overloading the DB.​
    - Mitigations: Two Ways 
	    - request coalescing/single‑flight (only one rebuilds while others wait) 
	    - cache warming (refresh hot keys before TTL expiry).​![[Pasted image 20260302191442.png]]
- **Cache consistenc**y: 
	- cache and DB can diverge (stale data); ![[Pasted image 20260302191654.png]]
	- Mitigation
		- invalidate on write
		- short TTLs
		- accepting eventual consistency where acceptable.​
- **Hot keys**: 
	- a single extremely popular key (e.g., Taylor Swift profile) overloads one cache node.​![[Pasted image 20260302191954.png]]
    - Mitigations: 
	    - replicate hot keys across nodes and/or 
	    - use local in‑process caches for very hot items

##### When to bring up caching 
1. Read heavy workload
2. Expensive Queries 
3. High DB CPU 
4. Latency Requirements 

##### How to introduce caching 
1. Identify the bottlenecks 
2. Decide what to cache 
3. Choose cache architecture 
4. Set an eviction policy 
5. Address the downsides 

###### How to talk about caching in an interview 
- Do not add a cache “by default”; first identify and quantify a bottleneck: read‑heavy workloads, expensive queries, high DB CPU, or strict latency requirements.​
- Once justified, explicitly state:
    - What you will cache and the cache keys.
    - The cache architecture (usually cache‑aside with an external cache like Redis).​
    - Eviction policy and TTL, with brief rationale.​
    - Relevant risks (stampede, consistency, hot keys) and how you’d mitigate the ones that actually matter for your design.​
- Caching usually shows up in deep dives on scale and latency; interviewers care more about your understanding and trade‑offs than memorizing pattern name


###### Redis
1. If Redis is single-threaded, how can it handle millions of requests per second?
	1. Because it avoids locking and context switching. Redis uses an event loop with non-blocking I/O, keeps everything in memory, and executes commands very fast. Single-threaded + in-memory + no locks = extremely high throughput. Nginx also uses same concept:
