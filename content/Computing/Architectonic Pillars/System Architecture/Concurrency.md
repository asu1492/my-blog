Related Notes 
1. [[Concurrency in Java]]

Why Concurrency 

What is Concurrency

How is Concurrency is achieved 

######  Three categories of concurrency problems 
1. Correctness
	1. Shared state get corrupted because two threads are trying to access it at the same time. 
	2. Solution : Make the operation atomic 
2.  Coordination 
	1. Work must be passed from one thread to another efficiently (e.g., producer–consumer).
	2. How consumer knows when work is arrived?
	3. What happens when work is arriving too fast for consumer?
3. Scarcity 
	1. Limiting access to finite resources 
	2.  Scarcity arises when there is a fixed limit of something: API rate limits, limited concurrent downloads, or a small pool of expensive resources like DB connections
	3. For counting-style limits (e.g., at most 10 concurrent calls), use a semaphore (a bucket of permits: acquire before work, release after work). Wrap usage in try/finally so permits are always returned even on exceptions
	4. For managing actual objects (e.g., a DB connection pool), use a blocking queue as a pool: pre-populate with N connections, each thread takes one, uses it, then puts it back, again ensuring return even on failures.

###### Correctness 
1. Check-then-act
	1. You first check a condition and then act later, leaving a gap where another thread can change the state (parking spots, tickets, rate limits, inventory). The fix is to make check+act an atomic section using a lock (synchronized in Java
2. Read-Modify-Write 
	1. Incrementing counters or balances is actually three steps (read, compute, write); two threads can lose updates. Use atomic variables for single counters (e.g., Java AtomicInteger) or locks when multiple values must stay consistent


Check-then-Act
```java
		public class BookingService {
			private final Object lock = new Object();
			public boolean bookSeat(String seatId, String userId){
				Seat seat = seats.get(seatId);
				if(seat==null)return false;
				synchronized(lock){
				if(seat.isAvailable()){
					seat.setOccupant(userId)
					return true;
				}
				return false;
			}
	     } 
	     
	     
	//Using Reentrant Lock
	private final ReentrantLock lock = new ReentrantLock();
	public boolean bookSeat(String seatId, String userId){
		Seat seat = seats.get(seatId);
		if(seat==null)return false;
		lock.lock();
		try{
			if(seat.isAvailable()){
				seat.setOccupant(userId);
				return true;
			}
			return false; 
		}
		finally{
			lock.unlock();
		}
	}
	
	//Better Design lock per seat
	private String occupant;
	public boolean book(String userId){
		lock.lock();
		try{
			if(occupant == null){
				occupant = userId;
				return true;
			}
			return false;
		} finally {
			lock.unlock();
		}
	}
	public boolean bookSeat(String seatId, String userId) {  
		Seat seat = seats.get(seatId);  
		return seat != null && seat.book(userId);  
	}
}

//Using Lock Free CAS
class Seat {  
	private final AtomicReference<String> occupant = new AtomicReference<>(null);  
  
	public boolean book(String userId) {  
		// CAS: if current value == null, set to userId  
		return occupant.compareAndSet(null, userId);  
	}  
	  
	public void cancel(String userId) {  
		// Only the same user can cancel  
		occupant.compareAndSet(userId, null);  
	}  
	  
	public boolean isAvailable() {  
		return occupant.get() == null;  
	}  
	  
	public String getOccupant() {  
		return occupant.get();  
	}  
}  
  
public class SeatService {  
	private final Map<String, Seat> seats = new ConcurrentHashMap<>();  
  
	public boolean bookSeat(String seatId, String userId) {  
		Seat seat = seats.get(seatId);  
		return seat != null && seat.book(userId);  
	}  
}
```
![[Pasted image 20260303071118.png]]

Read-modify-write
1. This pattern is visible whenever we are dealing with counters, seatAvailability, inventoryCount e.g.
2. Solution
	1. Carrying out at atomic level 
	2. At Hardware level - Compare and Swap - R-M-W in the single CPU cycle, thus guarantee from the hardware itself.

```java
private int count = 0; //Bug: R-M-W w/o protection 
private void increment(){
	count++;
}

private AtomicInteger count = new AtomicInteger(0);

public void increment(){
	count.incrementAndGet();
}
```

-----

###### Coordination 
1. Use a **blocking queue**: consumer calls take/get and sleeps when the queue is empty, automatically waking when producer calls put; make it bounded to avoid unbounded memory growth and to apply backpressure when producers are faster than consumers.

```java
//spin forever, wasting CPU cycles  
	while(true){
		if(!queue.isEmpty()){
			Task task = queue.poll();
			process(task);
		}
	}
	
// Adding sleep - Add latency to every task 
while(true){
	if(!queue.isEmpty()){
		Task task = queue.poll();
		process(task);
	}
	else{
		Thread.sleep(100);
	}
}

//Using Blocking Queue 
BlockingQueue<Task> queue = new LinkedBlockingQueue<>();

//Producers
public void handleSignup(User user){
	saveUser(user);
	queue.put(new EmailTask(user));
	return success();
}
//Consumer - worker thread
while(true){
	Task task = queue.take() //blocks when queue is empty, zero CPU 
	process(task);
}

//Using bounded blocking queue to prevent too many producers endangering the whole system
//Bounded Queue - blocks when full(backpressure)
BlockingQueue<Task> queue = new LinkedBlockingQueu<>(1000);

```

###### Scarcity

```java

Sempahore permits = new Semaphore(5); //max 5 concurrent 

public void download(String url){
	permits.acquire(); //grap the permit 
	try{
		doDownload(url);
	} finally {
		permits.release();
	}
}
```

When managing actual objects with state, create a poll with blocking queue. 
```java
BlockingQueue<Connection> pool = new LinkedBlockingQueue<>(1000);
for(int i=0; i<10; i++){
	pool.put(createConnection());	
}

//usage
public Result query(String sql){
	Connection conn = pool.take();
	try{
		return conn.execute(sql);
	} finally{
		pool.put(conn)
	}
}
```




###### Interview mental checklist
- Ask: Is there shared state across threads? If yes, think correctness issues, locks/atomics, check-then-act or read–modify–write.​
- Ask: Is work flowing from one thread to another? If yes, think coordination and bounded blocking queues.​
- Ask: Is there a fixed limit on some resource? If yes, think scarcity and apply semaphores or blocking-queue-based pools with guaranteed release in finally.​


---
##### Preventing & Detecting Deadlocks
###### You have a distributed transaction between two services. How do you prevent deadlocks?
1. Deadlocks in distributed transactions are prevented primarily using global lock ordering, where all services acquire locks in the same deterministic order based on resource identity. This removes circular wait. As a safety mechanism, we use timed locking with tryLock and fail fast if a lock cannot be acquired, releasing partial locks and retrying or compensating. This ensures the system never hangs.
	1. Lock Ordering (always acquiring locks in the same global sequence)
		1. Global Rule - Lock A -> Lock B -> Lock C 
		2. Global ordering removes the possibility of cycles. 
		3. Using logical ordering - e.g. customerId hash, shard IDs, resource IDs. However, order must be deterministic, known to all services and stable over time. 
	2. Timed Locking using tryLock(timeout, unit) from the ReentrantLock class to fail gracefully instead of hanging.
		1. TL does not prevent deadlocks structurally. It detects & escape them. 
		 java
			if (lock.tryLock(timeout, unit)) {
				try {
					// critical section
				} finally {
					lock.unlock();
				}
			} else {
				// fail gracefully
			}  
   
------


##### Concurrency in Nginx Server 
- Nginx handles 1M concurrent connections by using a few event‑loop workers with **epoll** and non‑blocking I/O, offloading heavy disk work to thread pools and relying on OS/kernel tuning for TCP and file descriptors.​
###### High‑level architecture
- Nginx is a reverse proxy and load balancer sitting between clients and many backend services, so clients talk only to Nginx.​
- It has one master process and multiple worker processes (often 1 per core) where each worker runs a single‑threaded event loop.​
- **Interview soundbite:** “Think of each worker as a highly efficient event loop that multiplexes thousands of connections instead of one thread per connection.”​

###### Why not threads per connection
- Thread‑per‑connection: 1M connections ⇒ ~1M mostly idle threads, huge context‑switch overhead and memory waste.​
- Thread‑pool + queue: limited threads, but most requests wait in queues, so latency explodes under very high concurrency.
- Nginx avoids both models; it lets the kernel own the blocking and only wakes up on I/O events.​

###### Role of kernel and epoll
- Kernel handles TCP handshakes, SYN/accept queues, and keeps per‑connection buffers and metadata; Nginx only sees a ready file descriptor.​
- Nginx sets sockets to non‑blocking and registers their file descriptors with epoll; each worker basically does `while (true) epoll_wait(...); handle events;`​
- epoll notifies when a socket is readable/writable or a new connection arrives, so a single worker can manage tens or hundreds of thousands of connections efficiently.​
- epoll scales because it’s event‑driven: the worker sleeps until the kernel has actual work, instead of spinning or context‑switching between idle threads.
###### Large files and disk I/O
- Blocking disk reads would stall the worker’s event loop and all its connections​
- Nginx offloads heavy disk I/O to a dedicated thread pool via async I/O directives, and can stream files in chunks using `sendfile` plus size limits​
- “Network I/O stays in the event loop; disk I/O gets pushed to a thread pool so the loop never blocks on slow storage.
###### Shared memory and rate limiting
- Workers share limited mutable state (caches, counters) via shared memory zones.​
- Simple rate limiting (per IP / per key) is implemented at Nginx, while complex auth and policies are often shifted to a downstream API gateway.​

###### Hitting 1M connections in practice
- You must tune OS limits: open files (nofile/ulimit), TCP backlog sizes, TCP connection limits, and buffer sizes.​
- Nginx config: set `worker_processes` (often `auto`) and large `worker_connections`, enable epoll, optimize sendfile/AIO, and then validate using load tools like `wrk` or `locust`​
- The real cap is hardware and kernel limits; Nginx’s event model is not the bottlenec​


list of concurrency problems I solved from leetcode. 1. Print in Order 2. FooBar 3. Print FooBar Alternately 4. Print Zero Even Odd 5. Building H2O 6. Traffic Light Controlled Intersection 7. Dining Philosophers 8. The Dining Philosophers (lock ordering variant) 9. Design Bounded Blocking Queue 10. Web Crawler Multithreaded 11. Fizz Buzz Multithreaded 12. Print Numbers Alternately 13. Semaphore Example (Zero Even Odd) 14. Producer Consumer (Bounded Buffer) 15. Thread Safe Counter 16. Concurrent HashMap (conceptual) 17. Read Write Lock Implementation 18. Barrier / Cyclic Barrier 19. Countdown Latch 20. Thread Pool Design You can go through the course “Java Multithreading for Senior Engineering Interviews“ to learn multithreading in deep.

