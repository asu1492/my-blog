
##### Reentrant Lock vs Synchronized 
![[Pasted image 20260303065212.png]]
```java

//Longest Waiting thread gets lock first. By default, non-fair, high throughput with no strict ordering. 
ReentrantLock fairLock = new ReentrantLock(true);

//Allow Thread to responds to interruptions 
try{
	lock.lockInterruptibly()
	try{
	}
	finally{
		lock.unlock();
	}
}
catch(InteruptedException e){
	Thread.currentThread().interrupt
}

//Timed Locking to avoid indefinite waiting 
if(lock.tryLock(2, TimeUnit.SECONDS)){
	try{
		//condition 
	}finally{
		lock.unlock();
	}
}
else{
	//Could not acquire lock 
}

//Multiple Condition 
privat final ReentrantLock lock = new ReentrantLock();
Condition condition = lock.newCondition();

lock.lock();
try{
	condition.await();
	condition.signal();
} finally{
	lock.unlock();
}
```

##### Reentrant Lock Implementation
```java

public class Counter{
	private final ReentrantLock lock = new ReentrantLock();
	print int count = 0;
	
	public void increment(){
		lock.lock();
		try{
			count++;
		}
		finally{
			lock.unlock();
		}
	}
	
	public int getCount(){
	return count;
	}
}
```



## Tier 1 — Must-Prepare (Almost Guaranteed)
1. How does `ConcurrentHashMap` work internally (Java 8+) and why is it preferred over synchronized maps?
	1.   +---------------------------+
         |     SynchronizedMap       |
         |   (e.g. HashMap wrapper)  |
         +---------------------------+
                    ^
                    |
               [ single lock ]
                    |
   -----------------+--------------------
   |                |                 |
Thread A         Thread B          Thread C
  get/put          get/put           get/put
       (must all take the same lock)
    2. ConcurrentHashMap (conceptual)
   hash(key) ──>   +--------+     +--------+     +--------+
                   | bucket | ... | bucket | ... | bucket |
                   |   0    |     |   1    |     |  N-1   |
                   +--------+     +--------+     +--------+
                       ^              ^              ^
                       |              |              |
                    lock 0         lock 1         lock k
                   (stripe)       (stripe)       (stripe)

	 1. If two threads collides on the same bucket, so they gets serialized without blocking the others. 
	 put(k, v)
		|
		v
		[1] Compute hash h = spread(k.hashCode())
		|
		v
		[2] Find index i = (n - 1) & h
		|
		v
		[3] If table[i] is empty:
				CAS(null -> newNode)  // lock-free fast path
			else:
				acquire bucket lock (or bin lock)
				walk list/tree at table[i]
				- if key exists: replace value
				- else: append / insert node
				release lock

2. Difference between `volatile`, `synchronized`, and atomic variables. When would you use each?
3. How does `ThreadPoolExecutor` work internally?
4. How do you size a thread pool for CPU-bound vs IO-bound workloads?
5. What happens when a thread pool queue is full?
6. Explain deadlock. How do you prevent and debug it in production?
7. Difference between optimistic and pessimistic locking. Where have you used them?
8. How do you prevent double processing in Kafka consumers?
9. How do you ensure thread safety in a Spring Boot REST API?
10. How do you handle concurrent updates to the same database record?

## Tier 2 — Very Likely (Strong Follow-ups)
11. What does `volatile` guarantee and what does it not guarantee?
12. Explain happens-before with a real example.
13. Why is double-checked locking broken without `volatile`?
14. Difference between `synchronized` and `ReentrantLock` in production systems.
15. What are rejection policies in `ThreadPoolExecutor`? Which one would you choose and why?
16. How does Kafka guarantee ordering and how does concurrency affect it?
17. How does `LongAdder` reduce contention compared to `AtomicLong`?
18. Difference between `HashMap`, `Hashtable`, and `ConcurrentHashMap`.
19. What is backpressure and how do you implement it?
20. How do you make an API idempotent?
    
## Tier 3 — Scenario & Design Driven (Senior Level)
21. Multiple requests try to reserve the same inventory item. How do you prevent overselling?
22. How do you design a thread-safe cache without hurting performance?
23. Where should concurrency be handled: application, cache, or database?
24. How do you handle concurrent cache refresh (cache stampede)?
25. How do you ensure exactly-once processing in distributed systems?
    
26. How do you debug thread pool exhaustion in production?
    
27. How do you detect and fix race conditions in live systems?
    
28. How do you throttle concurrent requests per user?
    
29. How do you handle flash-sale traffic spikes?
    
30. How do you design distributed locking?
    

---

## Tier 4 — Coding / Edge-Case Questions

31. Implement a thread-safe counter.
    
32. Implement a bounded blocking queue.
    
33. Fix a race condition in given code.
    
34. Implement a rate limiter.
    
35. Design a thread-safe LRU cache.
    

---

## Tier 5 — Tricky Knowledge Checks (Asked Selectively)

36. Is `SimpleDateFormat` thread-safe? Why?
    
37. Can two threads execute a synchronized method simultaneously?
    
38. What happens if an exception occurs inside a synchronized block?
    
39. Difference between `notify()` and `notifyAll()`.
    
40. Can deadlock occur with a single thread?
