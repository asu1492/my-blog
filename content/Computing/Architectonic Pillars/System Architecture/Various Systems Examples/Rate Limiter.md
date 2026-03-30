A rate limiter controls how many requests a client can make within a given timeframe to protect services from overload and abuse.

![[Pasted image 20260319130940.png]]

Requirement 
1. Functional
    Client Identification using unique key - API Key/UserID/IP address 
	Limit requests based on configurable rules 
	 Return Proper Error Header with Clear Response on Violation - 429 too many requests 
    Custom Rule - multiple config - per user, per endpoints, per tier(free/premium    
2. Non Functional
    Scalability 
	- 100M Daily Active User - 1M RPS 
	- millions - Sharding counters by client ID hash to spread counters; in-memory storage to avoid I/O bottlenecks ; 
    Availability and Consistency (CAP)
    1. RL operational even if nodes fails, thus availability >> consistency 
    2. Eventual consistency acceptable 
 .      Performance 
	 low latency rate limit checks with  <10ms 
    
    Reliability - under concurrent load, should enforce limit; handle abrupt traffic | use cache with atomic operations, Distributed Locks (e.g. Lua in Redis)
    Security - resist missuse  & integation with authentication layer; validate client identity before enforcing limit 
    Observability - allowed/rejected; rate-limt hit ratios; latency of limiter checks 
    Extensibility - plugin new limiting algo(token bucket, sliding window..); using Strategy pattern for algo selection 
    Failure Handling - Fail Open vs Fail Close 


Core Entities 
1. Requests 
2. Client (IP/UserId, API Key)
3. Rules (100 r/s)

System Interface 
isRequestAllowed(clientId, rulesId): {passes: boolean, remaining: number, resetTime: LocalDateTime}

----

1. Placement in architecture ?
2. How should we identify client ?


Placement in each microservice
1. in memory - fast
2. however, no global counter to keep track of total requests
![[Pasted image 20260319131318.png]]

Below Architecture solves global coordination problem. However, it will increase latency. 
![[Pasted image 20260319131442.png]]

Placing it in Gateway
1. Limits context - lesser info - only request info and header info 
2. Embed info related to user in JWT Token, e.g. premium/free tier user 
![[Pasted image 20260319131545.png]]

----
Functional Req 1: Client Identification
1. Maybe use combination
	- userId (authenticated user ) - higher rate limit 
	- Anon IP address - lower limit 

Functional Req 2 : Limit requests based on configurable rules 
 Fixed Window counter 
 1. TradeOff
	 1. Boundary Effect 
	 2. Starvation 
 ![[Pasted image 20260319132041.png|479]]
Sliding Window Log 
2. Implemented with a heap -> tons of memory 
![[Pasted image 20260319132150.png]]


 Sliding Window Counter - Approximation 
 ![[Pasted image 20260319132309.png]]
Token Bucket - resides in Redis that all API Gateways have access to  
1. TradeOff : Race Condition b/w two gateway - read after write -
	1. Reads from Redis and Write back to redis needs to happen atomically => LUA Scripting 
![[Pasted image 20260319132334.png]]
![[Pasted image 20260319132730.png]]



Rate Limiting Algo 
1. Fixed Window counter - burst at boundaries 
    e.g client can send 100 req at end of one window and 100 req at start of window -> around 200 request at boundary 
2. Sliding Window Log - most accurate, however memory heavy & high computation cost 
    - keep timestamp log of each request
    - On a new request
         remove timestamp older than currentime-windowSize
         count remaining timestamp, if within limit allow 
3. Sliding Window Counter - 
    - sub buckets 
        window - 60 seconds
        6 bucket - 10 seconds each 
    - count request per sub buckets 
    - new request, calculate sum of buckets overlapping the lat window duration 
    Sliding Window Counter reduces memory by grouping requests into time buckets but introduces slight approximation.
    For high-scale systems, Sliding Window Counter is more practical than Log.
4. Token bucket - controlled burst along with memory and processing efficient
    - Bucket Capacity = max request (e.g. 100 tokens)
    - Refill Rate (100 tokens/min)
    - each request consumes one token 
        if token avl, consume and allow request
        else reject with 429 


Functional Req 3:
1. Fail Fast - not using Queue 
2. 429 : too many 
3. Appropriate Header 
4. ![[Pasted image 20260319140927.png]]

----
Non Functional 

Redis : HMGET and HMSET - two operation -> 50k RPS 
1M / 50k = 20 instances 
We need 20 redis instances with shard 
Shard using clientId - using consistent hashing 
Practically Redis has Redis cluster that takes care of sharding the data using hashslot 


Availability 
If Redis node  goes down 
	Fail Open - RL is down - everything goes through 
	Fail Close  - RL is down - nothing goes through
	Fallback - Fixed Window - live with its drawback for sometimes, because without RL, failures are going to propagate downstream to other microservices. 
Never goes down - using replicas 

Latency 
Redis : network overhead - connection pooling - eliminate TCP handshake - resusing the same TCP connection pooling 
Geographic Clusters : Gateway and Redis closer to each other and together closer to end user. 


Zookeeper /etcd
Storing rules 
Import rules in memory & watch for changes
![[Pasted image 20260319142159.png]]
-------
High Level Data Flow 
Client -> Load Balancer -> Rate Limiter -> Cache/Store -> Application
1. Extract client identifier
2. Determine Applicable rule (global, per-endpoint, tier) - e.g. free, premium 
3. Queries Redis for Client Token bucket state (tokens and last refill time)
4. Token Bucket Logic 
    - how many new tokens to refill(based on time elapsed)
    - token exist, decrement and allow request
    - else, reject with 429 and headers like 'Retry-After'
5. Update Token state with TTL so counter expire automatically if unused. 


DS and Storage 
1. In memory cache store 
2. keys -> client+time window counters 
3. TTL 


----
When a request arrives at the API Gateway, we first extract the unique client identifier. 
We then check an in-memory counter (backed by Redis) using a token bucket algorithm. 
If the client hasn’t exhausted its tokens for the window, we allow the request and decrement tokens; 
otherwise, we return a 429 with reset headers. 
The counters are stored with TTL so they expire automatically, and we shard Redis based on client ID for scalability. 
Atomic operations ensure correctness under concurrency, and monitoring captures blocked request rates for observability.


Enum
public enum RateLimitingAlgo{
    TOKEN_BUCKET,
    FIXED_WINDOW,
    SLIDING_WINDOW
}

public enum ClientKeyType {
    API_KEY,
    USER_ID,
    IP_ADDRESS
}

Models/DTO

public class RateLimitRequest{
    private String clientId;
    private String endpoint;
    private ClientKeyType keyType;
}

public class RateLimitResponse{
    private boolean allowed;
    private int remaining;
    private Long resetTimeMillis;
}

Controller 
@RestController
@RequestMapping("/rate-limit")
public class RateLimitController {

    private final RateLimitService rateLimitService;

    public RateLimitController(RateLimitService rateLimitService){
        this.rateLimitService = rateLimitService;
    }

    @PostMapping("/check")
    public RateLimitResponse check(@RequestBody RateLimitRequest request){
        rateLimitService.checkRateLimit(request);
    }
}

Service 
@Service 
@RequiredArgsConstructor
public class RateLimitService{
    private final RateLimiterStrategyFactory strategyFactory ; 
    private final RateLimiterAuditRepository rateLimiterAuditRepository;

    public RateLimitResponse checkRateLimit(RateLimitRequest request){
     
       rateLimiterAuditRepository.save(
            new RateLimiterAudit(request.getClient(), request.getEndpoint(), request.isALlowed(), Instant.now())
       )
       RateLimitStrategy strategy = strategyFactory.getStrategy(request);  
       return strategy.allowRequest(req);
    }
}

Strategy Component Layer 
public interface RateLimiterStrategy{
    RateLimitResponse allowRequest(RateLimitRequest request);
}

@AllArgsConstructor
public class TokenBucketRateLimiter implements RateLimiterStrategy {
    
    private final JedisPool jedisPool;
    private final int bucketCapacity;
    private final double refillRatePerSecond;

    @Override
    public RateLimitResponse allowRequest(RateLimitRequest request){
       String key = "rate_limit:" + request.getClientId()
       long now = System.currentTimeMillis();

       try(Jedis jedis = jedisPool.getResouces()){
          Map<String, String> bucket = jedis.hgetAll(key);

          int tokens = bucket.cotainsKey("tokens") ? Integer.parseInt(bucket.get("tokens")) : capacity;

          long lastRefill = bucket.cotainsKey("lastRefill") ? Long.parseLong(bucket.get("lastRefill")): now;

          //refill
          double elapsedSecond = (now - lastRefill)/1000.0;
          int refill = (int) (elapsedSecond * refillRatePerSecond);

          tokens = Math.min(capacity, token + refill);

          boolean allowed = false;

          if(token > 0){
            token --;
            allowed = true
          }

          //Atomic Update using transaction
          Transaction tx = jedis.multi();
          tx.hset(key, "token", String.valuesOf(token));
          tx.hset(key, "lastRefill", String.valuesOf(now));
          tx.expire(key, 60);
          tx.exec();

          return new RateLimitResponse(allowed, tokens, now);

       } 
    }
}


JPA Layer for analytics/auditing 

@Entity 
public class RateLimiterAudit{
    @Id
    @GeneratedValue
    private Long Id;

    private String clientId;
    private String endpoint;
    private boolean allowed;
    private LocalDateTime timeStamp
}

public interface RateLimiterAuditRepository extends JpaRepository<RateLimiterAudit, Long> {}