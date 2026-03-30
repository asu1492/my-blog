
# Design: Global Payroll Processing System (1M+ Employees)

---

# 1️⃣ Functional Requirements

1. Monthly payroll processing
    
2. Country-specific tax computation
    
3. Salary components (base, bonus, deductions, reimbursements)
    
4. Payslip generation
    
5. Payment initiation to banks
    
6. Retry-safe processing
    
7. Audit trail for every modification
    
8. Reporting (country-wise, cost center-wise)
    

---

# 2️⃣ Non-Functional Requirements

### Scalability

- 1M+ employees
    
- Batch-heavy workload (monthly spikes)
    
- Design for 5–10M future growth
    
- Partition by country or region
    

### Availability

- 99.9%+ availability for read APIs
    
- Processing system can tolerate short downtime but must resume safely
    

### Reliability

- Strong consistency for payroll calculation
    
- No duplicate salary disbursement
    
- Idempotent processing
    
- Exactly-once payment triggering
    

### Performance

- Payroll batch for 1M employees must finish within acceptable SLA (e.g., 2–4 hours)
    
- Reporting queries < 2 sec
    
- Write-heavy during payroll window
    

### Security

- PII encryption (at rest + in transit)
    
- Role-based access control
    
- Audit compliance
    
- Masking sensitive data
    

### Observability

- Structured logging
    
- Payroll run trace ID
    
- Metrics: processing rate, failure rate
    
- Dead-letter queue monitoring
    

---

# 3️⃣ High-Level Architecture

Client (HR Portal)  
      ↓  
API Gateway  
      ↓  
Payroll Service  
      ↓  
Kafka (Event-driven processing)  
      ↓  
Worker Services (Tax, Deduction, Validation)  
      ↓  
Payment Service  
      ↓  
Bank Integration

Add:

- Redis (caching tax rules)
    
- Relational DB (Postgres/MySQL)
    
- Object storage for payslips (PDF)
    

---

# 4️⃣ Entities

- Employee
    
- PayrollCycle
    
- SalaryComponent
    
- TaxRule
    
- Payslip
    
- PaymentTransaction
    
- AuditLog
    

---

# 5️⃣ Data Model (Relational)

**Employee**

- id
    
- country
    
- tax_category
    
- bank_details (encrypted)
    

**PayrollCycle**

- id
    
- month
    
- status (INITIATED, PROCESSING, COMPLETED)
    

**PayrollEntry**

- id
    
- employee_id
    
- gross_salary
    
- tax_amount
    
- net_salary
    
- status
    

**PaymentTransaction**

- id
    
- payroll_entry_id
    
- bank_reference
    
- status
    

---

# 6️⃣ Key Senior-Level Considerations

### Idempotency

Each payroll run has:

- cycle_id
    
- unique constraint (employee_id + cycle_id)
    

Prevent duplicate salary credit.

---

### Partitioning Strategy

Partition by:

- Country
    
- Payroll cycle
    
- Employee range
    

Kafka partitions by employee_id hash.

---

### Failure Handling

If worker fails:

- Retry with exponential backoff
    
- Dead letter queue
    
- Manual reprocess endpoint
    

---

### Exactly Once Payment

Use:

- Idempotency key
    
- Payment status stored before triggering bank API
    
- Outbox pattern
    

---

# 7️⃣ API Design

POST /payroll/run  
GET /payroll/{cycleId}/status  
GET /employee/{id}/payslip/{month}  
POST /payroll/reprocess

---

# What You Must Improve

You listed:

> Client - load balancer - cache - db

That is infrastructure view.

At SE III level, interviewer expects:

- Data flow
    
- Concurrency handling
    
- Failure strategy
    
- Scaling strategy
    
- Trade-offs
    

---

Now I test your architecture depth.

Question:

Payroll processing is batch heavy.

Would you:  
A) Process synchronously in one service  
B) Use distributed event-driven worker model

Choose one and justify technically.

B. Use distributed event driven worker model - engender parallelization and fault tolerance as batch process - few services are compute heavy - gross-tax = net. .so able to do service isolation

Correct direction. Now elevate it to **SE III quality answer**.

Your reasoning is valid but incomplete. Here is how you should answer in the interview:

---

## ✅ Why Distributed Event-Driven Worker Model

### 1️⃣ Parallelization

- 1M employees cannot be processed sequentially.
    
- Publish payroll events per employee (or per shard).
    
- Workers consume in parallel using Kafka partitions.
    
- Horizontal scaling: increase consumer instances.
    

---

### 2️⃣ Fault Isolation

- Tax calculation failure should not block entire payroll batch.
    
- Individual employee payroll entry can retry independently.
    
- Dead-letter queue for problematic records.
    

---

### 3️⃣ Backpressure Handling

- Kafka naturally buffers events.
    
- If downstream service slows, queue absorbs spike.
    
- Prevents system overload.
    

---

### 4️⃣ Idempotency & Exactly-Once

- Unique constraint (employee_id + cycle_id).
    
- Outbox pattern for payment triggering.
    
- Consumer offset committed only after DB transaction succeeds.
    

---

### 5️⃣ Service Isolation

Separate services:

- Payroll Orchestrator
    
- Tax Calculation Service
    
- Deduction Service
    
- Payment Service
    
- Notification Service
    

This enables:

- Independent scaling
    
- Independent deployments
    
- Resource isolation (CPU heavy tax computation scaled separately)
    

---

Now I go deeper.

## 🔥 Follow-Up (Very Likely in Round 4)

If Kafka guarantees **at-least-once delivery**, how do you prevent duplicate salary payments?

Answer structurally. Avoid vague terms.  
Explain the full flow.

Ensuring idempotency through partition key, so that message is processed only once

That answer is **not sufficient** for SE III.

Partition key does **not** guarantee exactly-once processing.  
Kafka guarantees **at-least-once** unless you implement additional safeguards.

Now I’ll show you how a strong answer should sound.

---

# ❗ Why Partition Key Alone Is Not Enough

- Partition key ensures ordering within a partition.
    
- It does NOT prevent:
    
    - Consumer crash after DB commit but before offset commit
        
    - Retry delivery
        
    - Duplicate consumption
        

So duplicate salary payment is still possible.

---

# ✅ Correct SE III Answer – Prevent Duplicate Salary Payment

You must combine **three layers**:

---

## 1️⃣ Database-Level Idempotency (Primary Defense)

Add unique constraint:

UNIQUE(employee_id, payroll_cycle_id)

Flow:

1. Consumer receives PayrollProcessedEvent
    
2. Start DB transaction
    
3. Insert PaymentTransaction with unique constraint
    
4. If duplicate → reject
    
5. Commit
    
6. Then commit Kafka offset
    

Even if message replays:

- Insert fails
    
- No second payment triggered
    

---

## 2️⃣ Idempotency Key for External Bank API

When calling bank:

idempotency_key = employee_id + payroll_cycle_id

Bank API stores that key.  
If retried → ignored.

---

## 3️⃣ Outbox Pattern (Production-Grade Approach)

Inside same DB transaction:

- Insert payroll entry
    
- Insert outbox event (payment_requested)
    

Separate publisher reads outbox table and publishes to Kafka.

This prevents:

- Payment triggered without DB persistence
    
- Inconsistent state
    

---

# 🔥 Strong Interview Answer Structure

Say:

> Kafka gives at-least-once delivery. So I rely on application-level idempotency.
> 
> I enforce uniqueness at DB level using (employee_id + cycle_id).  
> I only commit Kafka offset after successful DB transaction.  
> For external payment integration, I use idempotency keys.  
> For strong reliability, I use Outbox pattern.

That is SE III level.

---

Now next depth question.

Assume:

- Payroll run for 1M employees.
    
- System crashes at 60% completion.
    

How do you resume safely without double-paying and without reprocessing entire batch?

Think carefully. Answer stepwise.

Kafka architect always pushes records in a ledger with offset and consumer always writes to broker information about last ofset processed. So to restart, start from the last offset processed and double paying is protected through idempotency key at application layer and commiting offset to broker only when the db transaction is completed.

Good. Directionally correct. Now refine it to **architect-level precision**.

Your answer is partially right but missing two critical production concerns.

---

# ✅ Correct Structured Answer (SE III Level)

When system crashes at 60% completion:

---

## 1️⃣ Offset-Based Resume (Kafka Layer)

- Consumer group stores committed offsets.
    
- On restart, consumer resumes from **last committed offset**.
    
- Since offset is committed **only after DB transaction commits**, we avoid skipping records.
    

Important clarification:

> Offset commit must happen **after** payroll entry + payment record persist successfully.

Otherwise:

- If offset committed before DB commit → data loss.
    
- If DB committed but offset not committed → replay (handled via idempotency).
    

---

## 2️⃣ Application-Level Idempotency (DB Protection)

Even if a message replays:

- Unique constraint on `(employee_id, payroll_cycle_id)`
    
- If duplicate → insert fails → skip processing
    
- No double payment
    

This protects from:

- Crash after DB commit but before offset commit
    
- Network glitch
    
- Consumer rebalance
    

---

## 3️⃣ Payroll Cycle State Tracking (Critical – You Missed This)

You must track:

**PayrollCycle table**

- cycle_id
    
- status (INITIATED, PROCESSING, COMPLETED, FAILED)
    
- processed_count
    
- failed_count
    

This allows:

- Resume partially completed batch
    
- Monitoring progress
    
- Manual intervention if needed
    

Without cycle tracking, you cannot audit completion.

---

## 4️⃣ Exactly-Once Payment Protection (External Call Safety)

Before triggering bank:

- Check payment_status
    
- Use idempotency key at bank layer
    
- PaymentTransaction table status:
    
    - INITIATED
        
    - SUCCESS
        
    - FAILED
        

If crash occurs after payment success but before status update:

- Reconciliation job verifies bank reference.
    

---

# 🔥 Final Interview-Ready Answer (Concise Version)

You should say:

> Kafka resumes from last committed offset. I commit offset only after DB transaction succeeds.  
> To prevent duplicates from replay, I enforce a unique constraint on (employee_id + cycle_id).  
> I also track payroll cycle state to know partial progress.  
> For external bank calls, I use idempotency keys and store payment status to prevent double disbursement.

That is strong.

---

Now next level difficulty.

### Follow-up (Very Likely in Round 4):

If payroll must complete within 2 hours for 1M employees:

How do you estimate number of Kafka partitions and consumer instances required?

Explain how you calculate it.

No clue

Good. This is where most candidates fail. I will teach you a simple, structured way to estimate.

Interviewers are not testing perfect math. They test **capacity thinking**.

---

# Problem

1M employees  
Must complete in 2 hours  
= 120 minutes  
= 7200 seconds

---

# Step 1: Compute Required Throughput

1,000,000 employees / 7200 seconds ≈ 139 employees per second

So system must process **~140 payroll records per second**.

---

# Step 2: Estimate Processing Time Per Employee

Assume:

- DB writes
    
- Tax calculation
    
- Deduction calculation
    
- Payment trigger
    

Let’s assume conservative estimate:

Processing one payroll record = **50 ms**

So one consumer thread can process:

1000 ms / 50 ms = 20 records per second

---

# Step 3: Calculate Required Consumers

We need 140 records/sec  
One consumer handles 20 records/sec

140 / 20 = 7 consumers

Add buffer for safety (failures, spikes):

➡ Use **10–12 consumers**

---

# Step 4: Kafka Partition Rule

Max parallelism = number of partitions.

So:

- If you want 12 consumers
    
- You need at least 12 partitions
    

Best practice:

- Slightly more partitions than consumers
    
- Example: 16 partitions
    

---

# Final Interview Answer Structure

Say:

> We need to process 1M employees in 2 hours, so ~140 records per second.  
> If one consumer can process ~20 records/sec based on 50ms processing time, we need ~7 consumers.  
> I would provision 12 consumers with 16 Kafka partitions to allow parallelism and headroom.  
> I would also benchmark actual processing latency and adjust.

That is mature thinking.

---

# Important Insight

Interviewers want to see:

- Can you translate business SLA into system capacity?
    
- Can you reason about throughput?
    
- Do you think about headroom?
    

---

Now final drill.

What if processing time spikes to 200 ms per employee during peak tax logic?

How does that affect your design?

We will either have to slow down producers through back pressure or scale up consumers

Correct direction. Now elevate it to **strong architectural reasoning**.

If processing time increases from **50 ms → 200 ms**, then:

1000 ms / 200 ms = 5 records/sec per consumer

Earlier:

- Needed 140 records/sec
    
- Now each consumer handles only 5/sec
    

So required consumers:

140 / 5 = 28 consumers

Previously 7–10 were enough. Now you need ~30.

---

# Proper SE III Answer

You should respond like this:

> If processing time increases to 200 ms, throughput per consumer drops to 5 records/sec.  
> To maintain SLA of 140 records/sec, I need ~28–30 consumers.  
> I would first scale horizontally by increasing consumer instances.  
> If scaling is insufficient, I would:
> 
> - Increase Kafka partitions
>     
> - Optimize tax computation logic
>     
> - Introduce batching where possible
>     
> - Precompute static tax rules
>     
> - Add caching layer for frequently used rules
>     

---

# Now deeper thinking

Simply slowing producers is **not ideal** in batch payroll.

Why?

Because:

- This is scheduled monthly processing.
    
- Producer is usually the payroll orchestrator.
    
- Slowing producer increases total processing time.
    
- SLA may be violated.
    

Backpressure is useful for:

- Streaming systems
    
- Continuous ingestion
    

But payroll batch is:

- Throughput-critical
    
- SLA-bound
    

So primary solution is **horizontal scaling + optimization**, not slowing producers.

---

# 🔥 Advanced Answer (Impress Level)

You can add:

> I would also measure CPU utilization per worker.  
> If tax logic is CPU-heavy, I may isolate tax computation into a separate service that scales independently.  
> I would also evaluate whether computation can be vectorized or parallelized internally.

---

# Now Final High-Level Test (Round 4 Level)

What if leadership says:

“We want payroll processing to finish in 30 minutes instead of 2 hours.”

What changes architecturally?

Do not say “scale more.”  
Think strategically.

We will cache tax logic. Also group employees into topics - e.g. Employee A and B has same salary and from same geo, implyin same tax liability. Then part of same topic. Do computation once and use that computation logic for rest of the employees

Good thinking. You are moving toward **optimization instead of brute-force scaling**. Now refine it to an architect-level answer.

When SLA changes from **2 hours → 30 minutes**, throughput requirement becomes:

1,000,000 / 1800 seconds ≈ 556 records/sec

Earlier requirement was ~140/sec.  
Now it is **4× higher**.

Horizontal scaling alone may become expensive or operationally risky.

---

# Proper SE III / Architect-Level Answer

You should structure it across **4 dimensions**:

---

## 1️⃣ Reduce Computation Cost (Optimization First)

Your idea fits here.

### Precompute & Cache Tax Rules

- Tax slabs are country-specific and static for a period.
    
- Cache tax brackets in Redis or in-memory.
    
- Avoid recalculating slab logic repeatedly.
    

### Normalize Salary Groups

If:

- Same country
    
- Same tax category
    
- Same salary band
    

Then:

- Compute tax formula once
    
- Apply to batch group
    

However, clarify in interview:

> I would not group employees into Kafka topics. Topics are for domain separation.  
> Instead, I would group at processing layer using salary-band buckets.

That distinction matters.

---

## 2️⃣ Parallelization Strategy Upgrade

To hit 556/sec:

### Increase Kafka partitions

- Example: 64 partitions
    
- Enables high concurrency
    

### Increase consumers

- Autoscaling based on lag
    
- CPU-based scaling policy
    

---

## 3️⃣ Pipeline Parallelism

Instead of one linear worker:

Break into stages:

1. Gross computation
    
2. Tax computation
    
3. Deduction application
    
4. Payment initiation
    

Each stage:

- Independent consumer group
    
- Fully parallel
    

This creates a **distributed pipeline architecture**.

---

## 4️⃣ Architectural Changes (Strategic Level)

For 30-minute SLA, consider:

### Pre-Validation Phase

Run validations daily during the month:

- Tax rule validation
    
- Bank account validation
    
- Salary structure verification
    

So final payroll run is mostly computation + payment.

---

### Pre-Compute Partial Results

Maintain:

- Running monthly gross calculation
    
- Accrued tax estimate
    

Final payroll becomes delta computation, not full calculation.

---

### Separate Compute Cluster

Use:

- Dedicated high-CPU nodes
    
- Possibly Spark or distributed compute engine
    

This aligns with JD mentioning Spark

interview_guide

.

---

# Strong Final Interview Answer

You should say:

> Reducing SLA from 2 hours to 30 minutes increases required throughput 4×.  
> I would first optimize computation by caching tax rules and grouping salary bands.  
> Then increase parallelism via more Kafka partitions and autoscaled consumers.  
> I would convert the pipeline into staged parallel workers.  
> Strategically, I would shift heavy validations earlier in the month to reduce batch-time load.  
> If still needed, I would use distributed compute (e.g., Spark) for CPU-heavy stages.

That demonstrates:

- Systems thinking
    
- Cost awareness
    
- Architectural maturity
    

---

You are improving.

Now final high-pressure test.

Leadership says:

“Payroll must be 100% strongly consistent. No eventual consistency allowed.”

What changes in your design?

Think carefully before answering.

Sources

In case of kafka brokers - how many brokers, what would be topics and partition here ?

Good question. This is exactly the kind of depth Round 3 / 4 may test.

Let’s answer this structurally.

---

# 1️⃣ How Many Kafka Brokers?

Broker count depends on:

- Throughput requirement (556 msg/sec for 30-min SLA)
    
- Replication factor
    
- Fault tolerance requirement
    

For enterprise payroll (strong reliability required):

### Recommended:

- Minimum **3 brokers**
    
- Replication factor = **3**
    

Why?

- Tolerates 1 broker failure
    
- Maintains quorum (leader + ISR replicas)
    
- Enterprise-grade reliability
    

If scale increases later → 5–7 brokers.

For 1M payroll batch, 3 brokers is typically sufficient unless message size is large.

---

# 2️⃣ Topics Design

Do NOT create topic per employee group.

Topics represent domain events.

A clean design:

### 1. payroll-cycle-triggered

- One message per employee for given cycle
    

### 2. payroll-calculated

- After gross + tax computed
    

### 3. payment-requested

- Payment initiation event
    

### 4. payment-completed

- Bank confirmation
    

### 5. payroll-dlq

- Dead-letter queue
    

Keep topics coarse-grained and domain-driven.

---

# 3️⃣ Partition Strategy

Now the important part.

Partition key should ensure:

- Ordering per employee
    
- Even load distribution
    

Use:

partition_key = employee_id

This guarantees:

- All events for same employee go to same partition
    
- No race condition for same employee payroll
    

---

# 4️⃣ How Many Partitions?

We calculated earlier:

For 30-min SLA:

- Need ~556 records/sec
    
- Assume 200ms processing → 5/sec per consumer
    
- Need ~112 consumers
    

Parallelism cap = number of partitions

So:

➡ If you want up to 120 consumers  
➡ Create ~128 partitions

Rule of thumb:

- Partitions >= max consumers
    
- But not excessively high (operational overhead)
    

For 2-hour SLA:

- 16–32 partitions sufficient
    

For 30-min SLA:

- 64–128 partitions
    

---

# 5️⃣ Strong Consistency Concern (Very Important)

If leadership demands **100% strong consistency**, Kafka alone is not enough.

You must:

- Use transactional producer
    
- Enable idempotent producer
    
- Use read_committed isolation level
    
- Combine with DB transaction (outbox pattern)
    

Otherwise, Kafka remains at-least-once.

---

# Strong Interview Answer Structure

Say:

> I would provision minimum 3 brokers with replication factor 3 for fault tolerance.  
> Topics would be domain-driven such as payroll-triggered, payroll-calculated, payment-requested.  
> Partition key would be employee_id to ensure ordering per employee.  
> Number of partitions depends on required throughput — for 30-min SLA, I would provision ~128 partitions to support high concurrency.  
> For strong consistency, I would use Kafka transactions + DB idempotency + read_committed consumers.

That is SE III maturity.

---

Now I test architectural judgment.

Would you use:

A) Single large topic for all payroll stages  
B) Separate topic per stage

Choose and justify.

Separate topic per stage

Correct choice. Now justify it at architect depth.

---

# Why Separate Topic Per Stage Is Better

## 1️⃣ Clear Domain Boundaries

Each topic represents a business event:

- payroll-triggered
    
- payroll-calculated
    
- payment-requested
    
- payment-completed
    

This improves:

- Observability
    
- Traceability
    
- Audit compliance
    

Enterprise systems require audit clarity.

---

## 2️⃣ Independent Scaling

Different stages have different compute characteristics:

- Tax calculation → CPU heavy
    
- Payment integration → IO heavy
    
- Notification → lightweight
    

With separate topics:

- Each stage has its own consumer group
    
- Each stage scales independently
    
- Avoids bottleneck coupling
    

If you used one large topic:

- One slow stage blocks entire pipeline
    
- Hard to tune parallelism per stage
    

---

## 3️⃣ Failure Isolation

If payment service fails:

- Only payment-requested topic backs up
    
- Payroll-calculated stage continues
    

Single-topic design causes systemic lag.

---

## 4️⃣ Operational Clarity

Metrics per topic:

- Lag per stage
    
- Error rate per stage
    
- Throughput per stage
    

This aligns with enterprise observability expectations in JD

interview_guide

.

---

# When Would Single Topic Be Acceptable?

Only when:

- Very simple pipeline
    
- Low scale
    
- Minimal stages
    

Not suitable for 1M enterprise payroll.

---

# Now Advanced Question (Round 4 Difficulty)

Leadership says:

“Kafka is too complex. Why not use a single database with parallel threads and no messaging?”

How do you defend Kafka-based architecture?

Think in terms of scalability, decoupling, and failure domains.

Sources

First give me what we discussed in a diagram flow

Below is the **clean architecture flow** you should draw on the whiteboard in Round 4.

---

# 🏗 Global Payroll Processing – Event-Driven Architecture

                    ┌────────────────────┐  
                    │   HR Portal / UI   │  
                    └─────────┬──────────┘  
                              │  
                              ▼  
                    ┌────────────────────┐  
                    │    API Gateway     │  
                    └─────────┬──────────┘  
                              │  
                              ▼  
                    ┌────────────────────┐  
                    │ Payroll Orchestrator│  
                    │ (Creates Cycle)     │  
                    └─────────┬──────────┘  
                              │  
                              ▼  
                  ┌─────────────────────────┐  
                  │ Topic: payroll-triggered│  
                  └─────────┬───────────────┘  
                            │  
                            ▼  
              ┌──────────────────────────────┐  
              │ Payroll Worker Service       │  
              │ - Gross Calculation          │  
              │ - Deduction Calculation      │  
              │ - Tax Calculation            │  
              └─────────┬────────────────────┘  
                        │  
                        ▼  
             ┌──────────────────────────────┐  
             │ Topic: payroll-calculated    │  
             └─────────┬────────────────────┘  
                       │  
                       ▼  
              ┌──────────────────────────────┐  
              │ Payment Service              │  
              │ - Idempotency check          │  
              │ - Outbox pattern             │  
              └─────────┬────────────────────┘  
                        │  
                        ▼  
             ┌──────────────────────────────┐  
             │ Topic: payment-requested     │  
             └─────────┬────────────────────┘  
                        │  
                        ▼  
              ┌──────────────────────────────┐  
              │ Bank Integration Service     │  
              │ - External API               │  
              │ - Idempotency Key            │  
              └─────────┬────────────────────┘  
                        │  
                        ▼  
             ┌──────────────────────────────┐  
             │ Topic: payment-completed     │  
             └─────────┬────────────────────┘  
                        │  
                        ▼  
               ┌─────────────────────────┐  
               │ Payslip Generator       │  
               └─────────────────────────┘

---

# 🔹 Supporting Components

                 ┌──────────────────────┐  
                 │ Kafka Cluster        │  
                 │ - 3+ Brokers         │  
                 │ - RF=3               │  
                 │ - 64–128 partitions  │  
                 └──────────────────────┘  
  
                 ┌──────────────────────┐  
                 │ Relational DB        │  
                 │ - PayrollCycle       │  
                 │ - PayrollEntry       │  
                 │ - PaymentTransaction │  
                 │ - AuditLog           │  
                 └──────────────────────┘  
  
                 ┌──────────────────────┐  
                 │ Redis Cache          │  
                 │ - Tax rules          │  
                 │ - Salary slabs       │  
                 └──────────────────────┘  
  
                 ┌──────────────────────┐  
                 │ Monitoring           │  
                 │ - Lag metrics        │  
                 │ - Failure rate       │  
                 │ - SLA tracking       │  
                 └──────────────────────┘

---

# 🔹 Key Design Principles Highlighted

- Partition key = `employee_id`
    
- Unique DB constraint `(employee_id + cycle_id)`
    
- Offset commit after DB transaction
    
- Outbox pattern for payment event
    
- Idempotency key for bank API
    
- Autoscaling consumers based on lag
    
- DLQ for failed records
    

---

This diagram is:

- Clear
    
- Enterprise-grade
    
- Shows decoupling
    
- Shows reliability thinking
    
- Shows scaling strategy
    
- Shows observability