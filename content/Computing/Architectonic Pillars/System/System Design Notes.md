![[Pasted image 20260228184559.png]]


## Core web system primitives (systems, components, client–server, proxies, data flow, DB types, APIs, caching, REST).
1. **Intro to System Design (Part 1)**
    - What is a system
    - what is design 
    - Why do we need system design
- Components of System Design 
	- Applications or services form the business logic layer, exposing ways to interact with the database through reads, writes, and processing of information
	- Communication protocols (like HTTP or TCP) provide the logical means for different machines and services to talk to each other over a network
	- Many systems include a presentation layer such as web apps, desktop apps, or mobile apps that users directly interact with, though some systems (like pure logging services) might not need a UI
	- These client interfaces send requests to backend applications, which in turn read and write data in databases, forming the basic request–response flow of a system


- **Parts 11–20**: Asynchronous communication, performance, reliability, scaling, replication, and CAP theorem.
- **Parts 21–26**: Sharding strategies and consistent hashing in detail.​
- **Parts 27–29**: System design interview foundations, capacity estimation tricks, and mistakes to avoid.

## Video-wise summaries (1–10)
1. **Client–Server Architecture (Part 3)**
    - Explains how clients send requests and servers respond over the network, and why this separation enables scalability.
    - Touches on multi-tier architectures (web server, app server, DB).
2. **Proxies (Part 4)**
    - Introduces forward and reverse proxies and where they sit in the request path.
    - Covers uses like load balancing, security, caching, and hiding internal topology.
3. **Data & Data Flow (Part 5)**
    - Discusses how data moves between components and how reads/writes propagate through a system.​
    - Highlights the need to reason about latency, throughput, and data paths end to end.
4. **Database Types: SQL, NoSQL, Column, Search, KV (Part 6)**
    - Compares relational, key–value, document, column, and search databases, and when each is a good fit.​
    - Emphasizes trade-offs in schema flexibility, consistency, query capabilities, and scaling.

## Anatomy of Applications and Services
- An application is software oriented around end‑user tasks or a full problem domain, typically integrating many capabilities and often including a user interface (e.g., web app, mobile app, desktop app).
- A service usually does a smaller, more specific job (e.g., payment, notification, search) and is primarily consumed by other software over an API, sometimes with no direct UI at all.

 How they relate in system design
- An application can expose and consume multiple services; for example, an e‑commerce site (application) calls inventory, payment, and recommendation services under the hood.
- Some software can be both: a mail server, for instance, is an application from an operator’s perspective but exposes SMTP/IMAP as services to other programs, which causes the conceptual blurring you noticed.

How to use the terms
- Use **“application”** when talking about the overall product or user‑facing unit (e.g., “the shopping application,” “the banking app”)
- Use **“service”** when talking about a focused capability or deployable backend component that other parts of the system call (e.g., “payment service,” “user profile service,” “search service”).

**Application Layer** 
The application (or app service) is responsible for implementing business workflows: 
1. checking permissions
2. validating inputs
3. orchestrating calls to databases and other services and 
4. deciding what data to return to the frontend. I

It does not worry about how buttons look or how pages are styled; instead, it ensures that when a user requests an action (such as placing an order or updating a profile), all the steps happen correctly and in the right order. This separation allows you to change the frontend framework or create multiple clients (web, mobile, internal tools) without rewriting the core business logic. 

It also supports clearer domain modeling, easier testing of rules, and more robust scaling strategies because you can independently scale the app layer based on load and complexity.

Service Layer
1. A service is a cohesive unit built around a specific capability or domain concept, such as "user management" or "notification delivery." It exposes a stable API or contract, hiding its internal implementation details (database, language, framework choices). This boundary allows other parts of the system to depend only on the contract, not on internal details. As a result, you can deploy, scale, or refactor the service independently, and teams can own and evolve services without tightly coupling to each other. This is true whether you are designing microservices, modular monoliths, or larger service-oriented architectures.
2. Services should encapsulate cohesive capabilities or domains where the data and behaviors are strongly related and tend to change for the same business reasons. For example, order management and payment processing might be separate services because they have different responsibilities, scaling needs, and change drivers, even though they interact. When boundaries follow real-world business concepts, cross-service communication patterns are easier to understand, and teams can own services more naturally.


Separation of Read and Write
1. enables different scaling strategies: read-heavy traffic can be handled with dedicated read replicas, caches, or specialized stores, while writes can focus on correctness and durability.
2. design write-side models to focus on validation, invariants, and capturing domain events, while read-side models are tuned for fast, straightforward queries that match how the UI or APIs need data.

![[Screenshot 2026-01-22 at 9.43.07 PM.png]]

![[Pasted image 20260122214505.png]]


## API – Application Programming Interface

Advantage of API's
1. Communication 
2. Abstraction - without callee worried about the internal change 
3. Platform agnostic 

Examples
1. Private API's
2. Public APIs
3. Web APIs - superset of public/private
4. SDK/Library API

Factors while designing API
1. API contracts - input and output  
2. Documentation 
3. Data Format 
4. Security [rate limiting and throttling]

API standards
1. RPC 
2. SOAP
3. REST 

API Contract
1. API contract in service design. The contract spells out how other services or clients can interact with a service: what endpoints or methods exist, what parameters they accept, what responses they return, and what error behaviors to expect. When this contract is stable and well-documented, other teams can integrate with confidence, and the owning team can change internal implementation details without breaking integrations. This pattern underpins loose coupling, independent deployment, and clear responsibility boundaries. Techniques like versioning and backward compatibility further help services evolve while respecting existing consumers that rely on earlier versions of the contract.
2. In the context of web system design, the service exposes HTTP endpoints such as /api/v1/users/userId/api/v1/users/userId. The contract documents which method to use (for example GETGET), which headers or query parameters are accepted, what the JSON request or response schema looks like, and which status codes can be returned. The client does not need to know how the service stores user data internally (SQL, NoSQL, cache, or another microservice). This decoupling allows teams to independently change database schema, internal caching, and business rules as long as the API contract remains consistent, reducing coupling and preventing breaking changes for consumers.
3.  The internal decisions—such as whether the service uses caching, microservices, message queues, or specific indexes—can change over time without affecting clients, as long as the contract remains stable.
4. This separation encourages loose coupling and clearer ownership boundaries between teams. It also enables safer refactoring, scaling, and performance tuning, because changes behind the API boundary do not ripple into client code.

---

 Codes
1.  When the client asks for order 123123 at /orders/123/orders/123, the server checks its storage and access rules. If there is no matching order, or if the client does not have rights to see it and the service chooses not to disclose that, it returns 404404. This allows clients to make decisions like stopping further retries, notifying users that the item is missing, or cleaning up local references. Separating 404404 from other error codes (such as 400400 for invalid requests or 500500 for server errors) is key to robust client behavior.
2.  It indicates that a new resource was successfully created and tells the client where that resource can be retrieved in future calls. In REST-style API design, a POSTPOST to a collection endpoint such as /feedback/feedback usually means "create a new feedback entry." When the server accepts this request and stores the data, it should confirm that outcome. 201201 signals that the creation succeeded, and the LocationLocation header points to the URI where the created resource lives, for example /feedback/789/feedback/789. This pattern allows clients to chain operations, such as immediately fetching the resource, linking to it in the UI, or later editing it. It also clearly distinguishes the creation scenario from other responses like 200200 (generic success) or 400400 (invalid input), improving both correctness and clarity.


## **Caching & Cache Patterns (Part 9)**

**What is a cache?**
- Fast storage layer that holds copies of data from a slower source (e.g., DB, disk, remote service)​
- Goal: reduce repeated computation/IO and improve latency for frequently accessed dat​a
**Intuition / toy example**
- Child asks a multiplication question; first time, you compute; next time you recall the previous answer instead of recomputing.​
- Same idea in systems: avoid re‑doing expensive work (DB queries, image fetches, heavy calculations) if result is already known.​
**Real‑world cache examples**

- Web app caching user profile / settings so each request doesn’t hit DB.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Caching images or static content near users so server is not asked every time.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Storing previous request–response pairs in memory to answer identical future requests quickly.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    

**Why isn’t data kept forever in cache? (staleness)**

- Underlying source data changes; cache can become stale if never removed or updated.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Need mechanisms so application does not keep serving outdated data indefinitely.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    

**What is cache invalidation?**

- Marking cached data as no longer valid when the underlying data changes.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Can be done by deleting keys, expiring them after some time (TTL), or explicitly refreshing them.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    

**What is cache eviction? Why do we need it?**

- Removing entries to free space when cache capacity is limited.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Needed because cache memory is finite; cannot store everything that was ever requested.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    

**Eviction policies (conceptual)**

- Least Recently Used (LRU): remove items that haven’t been accessed recently.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Other policies mentioned conceptually (e.g., remove less useful/older entries) to make room for new data.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    

**High‑level cache patterns (read/write behaviors)**

- Pattern idea: define how reads and writes interact with cache and DB.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Examples discussed:
    
    - Read from cache first; on miss, go to DB and populate cache (lazy loading).[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
        
    - Strategies around how writes are handled relative to cache and DB (e.g., write‑through, write‑back styles conceptually).[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
        

**Cache patterns – summary idea**

- Trade‑offs: freshness vs performance vs complexity.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- You choose a pattern based on how critical consistency is and how much extra latency you can tolerate on reads/writes.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    

**Where can a cache live? (placement)**

- In‑process / application‑level cache (inside the same process as the app).[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Distributed cache (separate cache service shared by multiple app instances).[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Placement affects latency, scalability, failure behavior, and consistency guarantees.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    

**Process vs distributed cache (implications)**

- Process cache: faster (no network hop), but each instance has its own copy and coherence is harder.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Distributed cache: central/shared view of cached data, easier to keep consistent across instances, but adds network hop and operational complexity.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    

**Key takeaways about using cache**

- Caching can drastically improve performance and reduce load on downstream systems.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    
- Must design invalidation + eviction + placement carefully to avoid bugs with stale data or inconsistent behavior.[[youtube](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)]​
    

---

### Quick self‑test (Make It Stick style)

Try to answer these without looking at the notes:

1. In one sentence, define a **cache** and its primary purpose.
    
2. Give one **human‑world analogy** of caching and one **system‑world example**.
    
3. Why can’t you just keep data in the cache forever? Name two reasons.
    
4. What is the difference between **cache invalidation** and **cache eviction** in your own words?
    
5. Describe a simple **read flow** using a cache in front of a database (steps from request to response).
    
6. Name two possible **locations** for a cache in a system and one trade‑off between them.
    

If you want, you can share your answers and they can be checked and refined before you move to the next video.



        
        
### REST APIs, CRUD, REST vs HTTP (Part 10)
- Clarifies what REST is (constraints and principles) versus just using HTTP as a transport.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Shows mapping between CRUD operations and HTTP verbs, and implications for statelessness and scalability.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​


**What is REST?**

- REST = Representational State Transfer, an architectural style for networked applications.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- Focuses on how clients and servers exchange representations of **resources** over a network, usually using HTTP.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    

**Core REST guidelines**

- Resources identified by URIs (e.g., `/books`, `/books/123`).[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- Use standard HTTP methods: GET, POST, PUT, DELETE for operations.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- Stateless communication: server does not store client session state between requests.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- Uniform interface: consistent, predictable patterns for resource access.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    

**Example REST API: book store**

- Resources: books, maybe authors, users, orders.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- Typical endpoints:
    
    - `GET /books` – list all books.
        
    - `GET /books/{id}` – get details of one book.
        
    - `POST /books` – create a new book.
        
    - `PUT /books/{id}` – update a book.
        
    - `DELETE /books/{id}` – delete a book.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
        

**CRUD vs HTTP methods**

- CRUD = Create, Read, Update, Delete.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- Typical mapping:
    
    - Create → POST (sometimes PUT).
        
    - Read → GET.
        
    - Update → PUT/PATCH.
        
    - Delete → DELETE.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
        

**State transfer vs stateless**

- “State transfer”: representations of resource state are sent in requests/responses (JSON, XML, etc.).[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- “Stateless”: each request contains all info needed; server doesn’t remember previous client requests or session state.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    

**Path vs query parameters**

- Path params: identify a **specific resource**; part of the URI structure, e.g., `/books/123`.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- Query params: modify/filter a request, e.g., `/books?limit=10&page=2&genre=fiction`.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- Use path for identity; use query for filters, pagination, sorting, options.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    

**HTTP responses and status codes**

- 2xx: success (200 OK, 201 Created).[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- 4xx: client errors (400 Bad Request, 401 Unauthorized, 404 Not Found).[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- 5xx: server errors (500 Internal Server Error).[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- Response body often includes data or error details in a consistent JSON structure.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    

**REST vs bare HTTP**

- HTTP is the **protocol** (methods, headers, status codes, etc.).[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- REST is an **architectural style** that uses HTTP conventions in a particular, resource‑oriented way.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    

**Security, auth, error handling**

- Need authentication/authorization (e.g., tokens) to protect APIs.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    
- Proper status codes + structured error responses help clients handle failures.[](https://www.youtube.com/watch?v=nUuAWn0AAiY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=10)​
    

---

## Quick self‑test (before watching)

Try answering without looking:

1. In one or two sentences, define **REST** and explain how it relates to HTTP.
    
2. For a “book” resource, write down full CRUD endpoints (URIs + HTTP methods).
    
3. Explain **statelessness** in REST: what must each request contain, and what does the server avoid storing?
    
4. When would you use a **path parameter** vs a **query parameter**? Give one example of each.
    
5. Map these to typical HTTP status codes:
    
    - Successful GET of a book.
        
    - Successfully created a new book.
        
    - Client sent malformed data.
        
    - Book ID not found.
        
6. Why is it useful to follow REST guidelines instead of inventing your own ad‑hoc API style?



## Message Queues & Producer–Consumer (Part 11)**
    

- Introduces queues to decouple producers and consumers and smooth traffic spikes.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Covers durability, at-least-once delivery, and typical use cases (email, background jobs).
    

## **Pub/Sub Messaging – Basics (Part 12)**

- Describes publisher–subscriber model where producers publish to topics and subscribers receive relevant messages.
- Highlights differences from simple queues (fan-out, loose coupling).
- publishers **only send messages to a topic or event bus** and do not know which subscribers will receive those messages or what they will do with them. This lack of direct awareness is what is meant by "decoupling." The **publisher’s only responsibility is to emit events** in a defined format; it does not care about the consumer’s identity, location, technology stack, or processing logic.\n\nThis decoupling has several benefits. It makes the system easier to scale, since you can independently scale publishers and subscribers based on their load. It improves flexibility, because you can add or remove subscribers without changing publisher code. It also increases resilience: if some subscribers fail, publishers can continue working, and the **broker can manage retries or dead-letter queues**. Overall, the model supports loosely coupled, event-driven architectures that are easier to maintain and extend over time.
- ub/Sub uses topics or event streams as intermediaries, so publishers can simply emit events and forget about who consumes them. This d**esign allows teams to introduce new consumers for existing events—such as logging, monitoring, analytics, or new features—without modifying the publisher**. It also permits independent scaling: a high-traffic subscriber can be scaled out without affecting publishers, and publishers can handle spikes in demand while subscribers process messages at their own pace. Furthermore, technology stacks can differ: one subscriber may be written in one language or framework, another in a different one, as long as they understand the message format.
- Publishers do not need to know which services are listening; they only need to decide which topic best matches the message. Subscribers in turn can subscribe to one or many topics depending on their responsibilities. This model supports clean separation of concerns, flexible routing, and the ability to add new subscribers to existing topics without altering the publishing services.
-  broker acts as the central infrastructure component that routes messages and manages how they are delivered. When a publisher sends a message to a topic, the broker looks up which subscriptions are interested in that topic and delivers copies of the message to each of them. The broker may also implement features like at-least-once delivery, ordering within a partition, or dead-letter queues for failed messages.\n\nBy centralizing routing and delivery semantics in the broker, individual services can stay focused on their business responsibilities. Publishers only need to know where to send messages (which topic or channel), and subscribers only need to subscribe to the topics they care abou
-  durable subscriptions or message retention combined with acknowledgments allow messages to survive temporary subscriber downtime. When a publisher sends a critical alert, the broker stores it in a durable topic or queue until a subscriber fetches and acknowledges it. If the subscriber is offline, the message remains in storage. **Once the subscriber is back, it can receive the backlog of alerts and process them**, preserving reliability.\n\n**Acknowledgments are also critical because they let the broker know which messages have been successfully processed.** If a message is not acknowledged within a certain window, the **broker can retry delivery or route it to a dead-letter mechanism for investigation.** This pattern is common in resilient Pub/Sub implementations and is especially important for systems handling alerts, financial events, or other high-value messages where loss is unacceptable.


13. **Pub/Sub Use Cases (Part 13)**

- Walks through practical scenarios for pub/sub: notifications, logging pipelines, event-driven microservices, etc.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Discusses pros/cons: scalability vs complexity and ordering issues.
    

14. **Performance Metrics (Part 14)**
    

- Defines high-level metrics like latency, throughput, availability, and error rate.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Shows how to use them to reason about system behavior under load.
    

15. **Performance Metrics of Components (Part 15)**
    

- Applies metrics to individual components (DB, cache, queues, services).[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Emphasizes identifying bottlenecks and planning optimizations systematically.
    

16. **Fault and Failure in Distributed Systems (Part 16)**
    

- Explains types of failures (node, network, partial failures) and why they are normal in distributed systems.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Introduces concepts like retries, timeouts, and redundancy to handle failures.
    

17. **Scaling in a Nutshell (Part 17)**
    

- Compares vertical vs horizontal scaling and their trade-offs.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Connects scaling strategies to typical system diagram changes (more replicas, stateless services).
    

18. **Database Replication (Part 18)**
    

- Explains master–replica setups, synchronous vs asynchronous replication, and read/write splitting.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Discusses consistency and lag trade-offs and failure handling.
    

19. **CAP: Consistency, Availability, Partitioning (Part 19)**
    

- Introduces CAP dimensions and what each means in practice.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Frames CAP as a way to think about trade-offs once partitions occur.
    

20. **CAP Theorem – Degrees & Use Cases (Part 20)**
    

- Explores how real systems choose points on the CAP spectrum rather than pure extremes.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Gives examples of CP vs AP style systems and when to favor one or the other.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    

## Video-wise summaries (21–26)

21. **Database Sharding – Basics (Part 21)**
    

- Defines sharding, logical vs physical shards, and motivations (scaling, isolation).[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Compares dynamic vs algorithmic sharding and when each approach fits.
    

22. **Key-based Sharding (Part 22)**
    

- Explains using a shard key and hash functions to distribute data.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Covers advantages (balance, simplicity) and disadvantages (rebalancing challenges).
    

23. **Range-based Sharding (Part 23)**
    

- Describes partitioning data by ranges (e.g., time or ID intervals).[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Mentions hotspots and when range-based sharding is beneficial.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    

24. **Directory-based Sharding (Part 24)**
    

- Introduces a directory or lookup service mapping keys to shards.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Explains flexibility vs added indirection and single-point concerns.
    

25. **Hashing & Need for Consistent Hashing (Part 25)**
    

- Recaps basic hashing and shortcomings when nodes are added/removed (massive remapping).[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Motivates consistent hashing as a way to minimize key movement.
    

26. **Basics of Consistent Hashing (Part 26)**
    

- Explains consistent hashing in plain language and ring-based mapping.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Shows how it helps with scaling caches or distributed storage with fewer data moves.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    

## Video-wise summaries (27–29)

27. **Foundation of System Design Interview (Part 27)**
    

- Starts with gathering functional vs non-functional requirements as the first step in interviews.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Emphasizes clarifying scope, constraints, and success metrics before drawing designs.
    

28. **Capacity Estimation – Thumb Rules (Part 28)**
    

- Teaches quick back-of-the-envelope calculations for QPS, storage, bandwidth, etc.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Offers shortcuts to reason about scale under time pressure in interviews.
    

29. **5 Mistakes in System Design Interviews (Part 29)**
    

- Shares common mistakes like skipping requirements, not stating assumptions, ignoring trade-offs, or poor communication.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    
- Provides guidance on structuring answers to avoid these pitfalls and show clear thinking.[youtube](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)​
    

If you want, a next step could be a one-page “cheat sheet” that maps these 29 videos to key formulas, definitions, and interview checklists.

1. [https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a](https://www.youtube.com/watch?v=FSR1s2b-l_I&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a)