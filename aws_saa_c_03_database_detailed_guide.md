# AWS Database Mastery Guide (SAA-C03)

Comprehensive, exam-focused notes with simplified diagrams and scenario questions.


---

## Amazon RDS

**Overview:** Managed relational DB for MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, and Aurora. Automates backups, patching, failover, and scaling.

**Key Features:** Multi-AZ (HA), Read Replicas (scaling/DR), automated backups + PITR, KMS encryption, IAM DB Auth (MySQL/PostgreSQL), Performance Insights, Parameter/Option Groups.

**Architectures:**

```text
        +--------- AZ-A ---------+        +--------- AZ-B ---------+
        |  App/EC2/ALB           |        |  App/EC2/ALB           |
        |         |              |        |                         |
        |      (writes/reads)    |        |                         |
        |         v              |        |                         |
        |   RDS Primary (Writer) |  <==>  |  RDS Standby (Sync)     |
        |        Endpoint        |  FAIL  |  Auto failover          |
        +------------------------+        +-------------------------+
Notes: Multi-AZ = High Availability (HA). One writer endpoint. Not for scaling reads.
```

```text
              Clients / App Tier
                      |
                Read/Write SQL
                      v
               RDS Primary (Writer)
                   /        \
          Async Read     Async Read
            Replica 1       Replica 2
 (Reads only endpoint)  (Reads only endpoint)
Notes: Read Replicas scale reads; promote for DR. Not automatic HA.
```

```text
Clients -> [ElastiCache Redis/Memcached] -> (Cache hit) -> Response
           |                                     ^
           v                                     |
         (Cache miss) ------------------> [RDS] (Query/Write)
Notes: Use cache to reduce DB load and latency. Redis supports persistence & pub/sub.
```

**Exam Qs:**
- Multi-AZ provides **HA**, not read scaling.
- Read Replicas provide **read scaling**, not automatic HA.
- For high IOPS OLTP → **io2**. For cost/perf → **gp3**.


---

## Amazon Aurora

**Overview:** MySQL/PostgreSQL-compatible relational DB built for cloud. Up to 5x MySQL, 3x PostgreSQL throughput. 6-way replicated storage across 3 AZs.

**Key Features:** Aurora Replicas (up to 15), Global Database, Serverless v2, Backtrack, Parallel Query.

**Architecture (Global DB):**

```text
Primary Region (Writer + Readers)  ==>  Read-Only Secondary Regions (Readers)
Sub-second replication; fast cross-region DR/failover.
```

**Exam Qs:**
- Global, low-latency reads → **Aurora Global Database**.
- Roll back without restore → **Backtrack**.


---

## Amazon DynamoDB

**Overview:** Serverless NoSQL key-value/document DB with single-digit millisecond latency at massive scale.

**Key Features:** On-Demand/Provisioned, DAX cache, Global Tables (active-active), Streams, TTL, PITR.

**Architecture:**

```text
[DynamoDB Table] --(Streams events)--> [Lambda] --> (Process/ETL/Notify)
        ^                                                         |
        |------------------- App reads/writes --------------------|
Notes: Serverless, event-driven patterns. Use DAX for read caching.
```

**Exam Qs:**
- Multi-region active-active → **Global Tables**.
- Microsecond reads → **DAX**.
- Change events → **Streams + Lambda**.


---

## Amazon ElastiCache (Redis/Memcached)

**Overview:** Managed in-memory cache for sub-ms latency.

**Key Features:** Redis (replication, persistence, pub/sub), Memcached (simple, multi-threaded), cluster mode sharding, Multi-AZ.

**Architecture:**

```text
Clients -> [ElastiCache Redis/Memcached] -> (Cache hit) -> Response
           |                                     ^
           v                                     |
         (Cache miss) ------------------> [RDS] (Query/Write)
Notes: Use cache to reduce DB load and latency. Redis supports persistence & pub/sub.
```

**Exam Qs:**
- Pub/Sub & persistence → **Redis**.
- Simple distributed cache → **Memcached**.


---

## Amazon Redshift

**Overview:** Managed, petabyte-scale data warehouse (OLAP). Columnar, MPP.

**Key Features:** RA3 w/ managed storage, Spectrum (query S3), Concurrency Scaling, AQUA.

**Architecture:**

```text
          +----------------------------+
          | Amazon Redshift Cluster    |
          |  - Columnar OLAP           |
          |  - RA3 managed storage     |
          +-------------+--------------+
                        |
                        | Spectrum (Query S3 directly)
                        v
                 [Amazon S3 Data Lake]
Notes: Use Spectrum to avoid loading data. Great for BI/Analytics at scale.
```

**Exam Qs:**
- Query S3 without load → **Spectrum**.
- Separate compute/storage → **RA3**.


---

## Amazon Neptune

**Overview:** Managed graph DB for highly connected data.

**Key Features:** Gremlin/SPARQL, up to 15 replicas, Multi-AZ, KMS encryption.

**Architecture:**

```text
[App/API] -> (Gremlin/SPARQL queries) -> [Neptune Cluster (Writer + Replicas)]
Notes: Use for graphs/relationships: social, fraud detection, recommendations.
```

**Exam Qs:**
- Graph traversals/relationships → **Neptune**.
- RDF queries → **SPARQL**.


---

## Amazon DocumentDB (MongoDB-compatible)

**Overview:** Managed document database compatible with MongoDB APIs.

**Key Features:** JSON/BSON docs, replicas, backups, KMS, VPC isolation.

**Architecture:**

```text
[App] -> [Amazon DocumentDB (MongoDB compatible)] <- TLS/KMS
Notes: Managed document store for JSON workloads needing MongoDB APIs.
```

**Exam Qs:**
- Need managed MongoDB API compatibility → **DocumentDB**.


---

## Amazon QLDB

**Overview:** Ledger DB with immutable, cryptographically verifiable journal.

**Use cases:** System of record, audit, supply chain.

**Architecture:**

```text
[App] -> [Amazon QLDB Ledger] -> [Immutable, cryptographically verifiable journal]
Notes: System of record with full history & audit trail.
```

**Exam Qs:**
- Tamper-evident history → **QLDB**.


---

## Amazon Timestream

**Overview:** Serverless time-series DB for IoT/metrics.

**Key Features:** Memory/magnetic tiering, rollups, SQL-like queries.

**Architecture:**

```text
[IoT/Telemetry] -> [Amazon Timestream] -> (Memory store -> Magnetic store)
Notes: Purpose-built time-series DB with built-in rollups, tiering, SQL-like queries.
```

**Exam Qs:**
- Time-series telemetry at scale → **Timestream**.
