
### 1. CPU Clock Speeds Are Barely Increasing

For many years, performance improvements came from increasing the **CPU clock frequency** (GHz). A higher clock meant more instructions per second.

However, physical and thermal limits (power consumption, heat dissipation) have largely stopped significant increases in clock speeds. Most CPUs have stayed roughly in the **3–5 GHz range** for years.

Implication:
- Single-thread performance improvements are limited.
- Programs cannot rely on faster CPUs automatically improving performance.

---

### 2. Multi-Core Processors Are Standard

Instead of increasing clock speed, hardware manufacturers add **more cores** to CPUs.

Examples:
- Laptop CPUs: 8–16 cores
- Servers: 32–128 cores or more

Each core can execute tasks independently.

Implication:
- Software must use **parallelism** (multiple threads or processes) to utilize hardware fully.
- Programs that remain single-threaded cannot benefit from additional cores

---

### 3. Networks Are Getting Faster

Network bandwidth has improved significantly:

Typical progression in data centers:

- 1 Gbps → 10 Gbps → 25 Gbps → 100 Gbps+
    

This reduces the cost of communication between machines.

Implication
- Systems increasingly use **distributed architectures**.
- Work can be spread across many machines efficiently.

---

### 4. Combined Effect: Parallelism Will Increase

Because of these trends:

|Hardware Trend|Software Consequence|
|---|---|
|Clock speed plateau|Single-thread scaling limited|
|More CPU cores|Need multi-threaded programs|
|Faster networks|Distributed systems become viable|

Result:

- Systems rely on **parallel execution** across cores and across machines.
    

Examples in modern systems:

- Distributed databases
- MapReduce-style processing
- Microservices
- Stream processing framework

---

### 5. Why This Matters for Distributed Systems

The book emphasizes that modern applications must deal with:

- **Concurrency**
- **Parallel processing**
- **Distributed coordination**
- **Data consistency across nodes**

This shift is why distributed systems concepts (replication, partitioning, consensus, fault tolerance) are central to modern software architecture.