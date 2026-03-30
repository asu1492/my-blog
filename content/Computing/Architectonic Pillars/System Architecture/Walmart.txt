1. Rate Limiter (LLD + distributed)
2. Kafka exactly-once design
3. Idempotency key implementation
4. Spring Transaction management
5. Caching with Redis
6. Concurrency in Java (ExecutorService, CompletableFuture)


Prepare these:
Rate Limiter
Feed service
Reservation System
Payment Processing
Notification System
Order Management
Cache System

Practice 2–3 complete designs on: Rate Limiter, Feed service, and Notification service, going down to class level (interfaces, models, repositories, services, concurrency).
For each design, be ready with: APIs, DB schema (SQL + one NoSQL angle), caching plan (e.g., Redis with LRU/LFU), and where Kafka fits in.
Rehearse Java/Spring Boot specifics: transactions, exception handling, validation, testing (JUnit/Mockito), and packaging for a microservice.
​


1️⃣ Rate Limiter (Very High Probability)
Why important:
Covers concurrency
Covers caching (Redis)
Covers distributed systems
Covers thread safety
Concepts reused in:
    API gateway
    Payment system
    Notification throttling
    Order processing
    Fraud detection
Prepare:
    Fixed window
    Sliding window
    Token bucket
    Distributed Redis implementation

3️⃣ Payment Processing System
Why:
    Idempotency
    Transactions
    State machine
    Retry logic
    Distributed consistency
    Exactly-once semantics

Reusable in:
    Order system
    Booking system
    Refund flow
    Wallet service

Prepare:
    Payment state transitions
    Idempotency key
    @Transactional usage
    Retry pattern
    Failure handling

Feed service

2️⃣ Notification System (You Already Practiced)
Reusable concepts:
    Strategy pattern
    Async processing (Kafka)
    Retry + DLQ
    Delivery tracking
    Persistence
    Authorization
    Batch processing

Reusable for:
    Email service
    Alerting system
    Messaging system
    Event dispatcher
    Audit system


4️⃣ Order Management System
Why:
    State machine design
    Microservice thinking
    Event-driven design
    Saga pattern

Reusable in:
    Shipment tracking
    Inventory system
    Workflow engines


5️⃣ Cache System (Redis-based)
Concepts:
    Cache-aside
    Write-through
    Write-back
    TTL
    Cache invalidation

Reusable in:
    Rate limiter
    User session store
    Product lookup
    Configuration service


Reusable Core Building Blocks (Master These); If you master these, any LLD becomes composable.
1. Controller → Service → Repository layering
2. Strategy pattern
3. Factory pattern
4. State machine modeling
5. Idempotency pattern
6. Retry + DLQ
7. Optimistic locking
8. Kafka producer/consumer flow
9. Thread safety & synchronization
10. Caching patterns



State Machine Modeling 
1. A system where entity transitions b/w well defined states based on events 
State + Event -> Next State 
E.g. Order Flow : CREATED -> PROCESSING -> SHIPPED -> DELIVERED -> RETURNED -> REFUND 
2. I model state transitions explicitly to prevent invalid business operations and maintain system consistency.
2. Model
    State Enum
    Transition Rules 
    Validation before Update

    //payment state machine
    public enum PaymentStatus{
        CREATED,
        PROCESSING,
        SUCCESS,
        FAILED,
        CANCELLED
    }
    //enforcing transition 
    public class Payment{
        private PaymentStatus status;

        public void process(){
            if(status!=PaymentStatus.CREATED){
                throw new IllegalStateException("Invalid transition);
            }
            status = PaymentStatus.PROCESSING;
        }

        public void markSuccess(){
            if(status!=PaymentStatus.PROCESSING){
                throw new IllegalStateException("Invalid transition);
            }
            status = PaymentStatus.SUCCESS;
        }
    }

    public boolean canTransition(PaymentStatus current, PaymentStatus next) {
        if (current == CREATED && next == PROCESSING) return true;
        if (current == PROCESSING && 
            (next == SUCCESS || next == FAILED)) return true;
        if (current == SUCCESS && next == REFUNDED) return true;

        return false;
    }

    public void updateStatus(Payment payment, PaymentStatus newStatus) {
        if (!canTransition(payment.getStatus(), newStatus)) {
            throw new IllegalStateException("Invalid transition");
        }

        payment.setStatus(newStatus);
    }

Idempotency Pattern
Idempotency ensures safe retries by using a unique request identifier stored with transaction state.
1. Same request executed multiple times -> produces same result.
2. E.g. client retries payment due to timeout, do not charge twice. 
3. Unique key Prevents race condition.
3. How? 
    - Client sends Idempotency key
    {
        "amount": 1000,
        "idempotencyKey": "txn-123"
    }

        payment_table
        -------------
        id
        idempotency_key (unique)
        status
        amount
    @Transactional
    public Payment createPayment( PaymentRequest request){
        Optional<Payment> existing = paymentRepository.findByIdempotencyKey(request.getKey());

        if(existing.isPresent()){
            return existing.get()
        }

        Payment payment = new Payment(...);
        payment.setIdempotencyKey(request.getKey())
        return paymentRepository.save(payment);
    }

Retry & Dead Letter Queue (DLQ)
Retry handles transient failures, DLQ isolates permanent failures without blocking processing pipeline.
1. If retries exceed threshold → send to separate topic(dead letter topic)
2. Why DLQ 
    Avoid infinite retry loop
    Manual inspection
    Alerting
3. kafa config 
    spring.kafka.consumer.max-attempts=3
    spring.kafka.consumer.dlq-topic=notification-dlt


Optimistic Locking
1. Used to prevent lost updates in concurrent DB writes.
    orders
    ------
    id
    status
    version
    Both UserA and UserB reads version 1. UserA updates Status and now version changes to 2. 
    UserB tries to update Status results in OptimisticLockException and second update fails.
2. 