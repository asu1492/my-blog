# YARN — Yet Another Resource Negotiator

YARN is the resource management and job scheduling layer of the Hadoop ecosystem (Hadoop 2.x+). It decouples resource management from the MapReduce programming model, allowing multiple computation frameworks (Spark, Tez, Flink) to share the same cluster.

---

## Architecture

```
┌─────────────────────────────────┐
│         Resource Manager        │  ← Global scheduler (one per cluster)
│  ┌─────────────┐  ┌──────────┐  │
│  │  Scheduler  │  │  Apps    │  │
│  │  (pluggable)│  │  Manager │  │
│  └─────────────┘  └──────────┘  │
└────────────┬────────────────────┘
             │ allocates containers
    ┌────────┴────────┐
    ▼                 ▼
Node Manager      Node Manager       ← Per-node agents
(CPU / RAM        (CPU / RAM
 tracking)         tracking)
    │                 │
Application       Application
Master            Master             ← Per-job coordinators
```

| Component | Role |
|-----------|------|
| **Resource Manager (RM)** | Global authority for cluster resources. Contains the Scheduler (allocates containers) and the Applications Manager (accepts job submissions, launches AMs). |
| **Node Manager (NM)** | Runs on each worker node. Reports available CPU/RAM to RM; launches and monitors containers. |
| **Application Master (AM)** | Per-application process. Negotiates resources from RM, coordinates tasks on NMs. MapReduce, Spark each have their own AM. |
| **Container** | Unit of allocation — a bundle of CPU cores + memory on a specific node. |

---

## Resource Scheduling

Three built-in schedulers:

| Scheduler | Behaviour |
|-----------|-----------|
| **FIFO** | Simple queue; long jobs block short ones |
| **Capacity** | Partitions cluster into queues with guaranteed capacity; suited for multi-tenant orgs |
| **Fair** | Dynamically balances resources so all apps get equal share over time |

---

## Key Concepts

- **Container** — smallest schedulable unit; YARN does not care about what runs inside.
- **AM heartbeat** — AM regularly heartbeats to RM to request more containers or signal completion.
- **Resource Locality** — YARN tries to schedule containers on the node where the input data block lives (data locality).
- **Node Labels** — partition nodes into groups (e.g., GPU nodes) for targeted scheduling.

---

## YARN vs Spark Cluster Manager

Spark can use YARN as its cluster manager (spark-submit `--master yarn`). In that case:
- Spark Driver = Application Master
- Spark Executors = YARN Containers

See: `[[Apache Spark/Spark Cluster Manager]]`

---

## Related

- `[[Hadoop and MapReduce]]` — original compute framework YARN replaced MR1's JobTracker
- `[[Apache Spark/Apache Architecture]]` — driver/executor model runs on top of YARN
- `[[Data Pipelines]]` — Airflow submits YARN jobs as operators
