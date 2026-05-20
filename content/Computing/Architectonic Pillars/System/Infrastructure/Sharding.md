# Sharding

**Summary**: Covers Sharding in system infrastructure, including key concepts, trade-offs, and practical usage.
**Tags**: #system-design #infrastructure #sharding
**Created**: Unknown
**Last Updated**: 2026-05-20

---

---

## Why do we need Sharding

A single database node has physical limits — storage, RAM, CPU, and network throughput. As data grows:

- **Read/write throughput** hits the ceiling — a single machine can only handle so many queries per second.
- **Storage** overflows — a single disk or node can't hold hundreds of TBs.
- **Latency** degrades — large tables mean slower queries even with good indexes.
- **Single point of failure** — one node going down takes everything down.

Vertical scaling (bigger machine) buys time but has a hard ceiling and is expensive. **Sharding is horizontal scaling** — split the data across many machines so each handles a fraction of the total load.

---

## What is Sharding

Sharding is the practice of **partitioning a large dataset across multiple independent database nodes (shards)**, where each shard holds a disjoint subset of the data.

```
          All Users
         /    |    \
      Shard 0  Shard 1  Shard 2
     userId    userId   userId
      0–33%    33–66%   66–100%
```

Key properties:
- Each shard is a **full, independent database** — it has its own storage, compute, and (optionally) replicas.
- A **shard key** determines which shard a record belongs to.
- Queries that include the shard key are routed to exactly one shard (**shard-local**).
- Queries without the shard key must hit all shards (**scatter-gather**) — expensive.

---

## How Sharding is Implemented

### 1. Choose a Shard Key

The most important decision. A good shard key:
- Has **high cardinality** (many distinct values) — avoids all data going to one shard.
- Is **evenly distributed** — avoids hot shards.
- Is **present in most queries** — avoids scatter-gather.

| Common shard keys | Use case |
|---|---|
| `userId` | User-centric data (feed, orders, profile) |
| `tenantId` | Multi-tenant SaaS |
| Geographic region | Latency-sensitive, data residency |
| `createdAt` range | Time-series / analytics |

---

### 2. Sharding Strategies

#### Range-Based Sharding
Assign ranges of the shard key to shards.

```
Shard 0: userId 1 – 1,000,000
Shard 1: userId 1,000,001 – 2,000,000
```

- ✅ Simple, range queries are efficient.
- ❌ Uneven distribution if data is skewed (e.g., most users are new → Shard 2 is hot).

#### Hash-Based Sharding
Apply a hash function to the shard key; use modulo to assign a shard.

```
shard = hash(userId) % numShards
```

- ✅ Uniform distribution.
- ❌ Range queries require scatter-gather. **Resharding is painful** — adding a node changes all assignments.

#### Consistent Hashing
Place shards on a ring; each key maps to the nearest shard clockwise. Adding/removing a shard only remaps a fraction of keys.

```
Ring: [Shard A] ──── [Shard B] ──── [Shard C] ──── [Shard A]
key → hash → position on ring → nearest shard clockwise
```

- ✅ Minimal data movement on reshard.
- ✅ Used by: Cassandra, DynamoDB, Redis Cluster.
- ❌ Can cause uneven load without virtual nodes.

**Virtual nodes (vnodes):** Each physical shard occupies multiple positions on the ring, improving balance.

---

### 3. Routing Layer

The application (or a proxy) must know which shard to contact.

| Approach | How | Example |
|---|---|---|
| **Client-side routing** | App hashes key, connects to correct shard | Redis Cluster clients |
| **Proxy routing** | Proxy intercepts query, forwards to correct shard | Vitess, ProxySQL |
| **Directory / metadata service** | Lookup table maps key ranges to shards | HBase, custom solutions |

---

### 4. Resharding

Adding shards after launch is the hardest operational problem:
1. Pick a new shard count.
2. Re-hash all keys → compute new shard assignment.
3. Migrate data from old shards to new shards (online or offline).
4. Update routing layer atomically.

Consistent hashing minimises the data that needs to move.

---

## Common Problems

| Problem | Description | Solution |
|---|---|---|
| **Hot shard** | One shard gets disproportionate traffic (e.g., a celebrity user) | Add a random suffix to the shard key; route hot keys to dedicated shards |
| **Cross-shard joins** | Queries spanning multiple shards are slow | Denormalize; co-locate related data on the same shard |
| **Cross-shard transactions** | ACID across shards requires distributed transactions | Avoid if possible; use eventual consistency + saga pattern |
| **Uneven growth** | One shard fills up faster | Use smaller, more numerous shards; re-balance with consistent hashing |

---

## How to talk about Sharding in an interview

- Don't reach for sharding immediately. First exhaust: indexing, query optimisation, read replicas, caching.
- When justified, state: the shard key and why you chose it, the strategy (hash vs range vs consistent hashing), and how routing works.
- Address the hotspot problem explicitly — interviewers will probe it.
- Mention that cross-shard transactions are avoided by design (co-locate data that changes together).

---

## Related Notes
- [[Caching]] — often the first line of defence before sharding is needed
- [[Consistency Model ACID, BASE]] — sharding typically pushes you toward BASE; understand the trade-off
- [[Message Queue]] — fan-out writes across shards can be routed via a queue
- Used in: [[Rate Limiter]] · [[FeedService]]
