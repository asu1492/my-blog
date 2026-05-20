# Introduction to DS

**Summary**: Covers Introduction to DS in distributed systems, including foundational ideas and practical trade-offs.
**Tags**: #system-design #distributed-systems #introduction-to-ds
**Created**: Unknown
**Last Updated**: 2026-05-20

---

Why DS
1. Parallelism 
2. Fault Tolerance [How: Non Volatile Storage, Replication]
	1. Availability 
	2. Recoverability 
3. Physical 
4. Security - Isolated 

Challenges of DS
1. Concurrency 
2. Partial Failures 
3. Performance 

MapReduce
Raft for Fault Tolerance 
K/V Servers
Sharded K/V Services 


Infra
1. Storage
2. Communication 
3. Computation [e.g MapReduce]

Abstraction 
1. Implementation - RPC, Threads, Concurrency Locks 

Performance 
1. Scalability -> 2x computers -> 2x throughput 

Consistency 
1. put(k,v)
2. get(k) -> v [Danger of Replication, may not get updated value of the key]
3. Type
	1. Strong Consistency -> Expensive 
	2. Weak/Eventual Consistency 


MapReduce 
1. ![[Pasted image 20260510210616.png]]
2. Whole Computation is called job
3. Each Map and Reduce are called Task, e.g. Map task, Reduce Task 
4. MAP FUN ![[Pasted image 20260510210829.png]] 
5. Reduce Fun 
	1. Reduce(k,v) -> emit(len(v))
6. GFS (Google File System) and Map Worker stayed on the same machine, but Reduce ran on a different m/c communicating over a network.

---

## Related Notes

- [[00. Master Knowledge Map]]
- [[System/00. Overview|System Overview]]
