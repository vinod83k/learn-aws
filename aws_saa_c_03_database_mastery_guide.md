
# AWS Database Mastery Guide (SAA-C03)

This guide covers all important AWS database services relevant for the AWS Solutions Architect – Associate (SAA-C03) exam.  
Each section includes **Overview → Key Features → Architectures → Exam Questions**.

---

## 1. Amazon RDS (Relational Database Service)

### Overview
Amazon RDS is a managed relational database service that supports MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, and Amazon Aurora.

### Key Features
- Multi-AZ Deployment for High Availability
- Read Replicas for Read Scaling and DR
- Automated Backups and Snapshots
- Storage Types: gp2/gp3, io1/io2, Magnetic (legacy)
- Security with KMS, IAM Authentication, VPC Security Groups
- Performance Insights, ElastiCache integration

### Architectures
- RDS Multi-AZ (High Availability)
- RDS Read Replicas (Scalability)
- RDS + ElastiCache (Performance)
- RDS + Lambda (Automation)

### Exam Questions
1. Which feature ensures automatic failover? → **Multi-AZ Deployment**
2. Which feature offloads reporting queries? → **Read Replicas**
3. Which is best for high-transaction SQL Server? → **io2 Provisioned IOPS**
4. How to encrypt RDS Oracle at rest? → **KMS + Option Groups**

---

## 2. Amazon Aurora

### Overview
Aurora is a MySQL and PostgreSQL-compatible relational database engine built by AWS, offering up to 5x the performance of MySQL.

### Key Features
- Distributed storage across 3 AZs
- Aurora Replicas (15 max)
- Global Database for multi-region replication
- Serverless v2 for scaling
- Backtrack for time rewind

### Architectures
- Aurora Multi-AZ clusters
- Aurora Global Database
- Aurora + Lambda (serverless event-driven apps)

### Exam Questions
1. Which supports global replication? → **Aurora Global Database**
2. Which scales to 15 replicas? → **Aurora Replicas**
3. Which rewinds DB state? → **Aurora Backtrack**

---

## 3. Amazon DynamoDB

### Overview
DynamoDB is a fully managed NoSQL database offering single-digit millisecond performance at scale.

### Key Features
- Global Tables (multi-region, active-active)
- On-Demand vs Provisioned capacity
- DAX (caching)
- Streams for event-driven apps
- TTL for item expiration

### Architectures
- DynamoDB + Lambda (serverless)
- DynamoDB Streams + Kinesis
- DynamoDB + S3 for analytics export

### Exam Questions
1. Which feature provides global multi-region replication? → **Global Tables**
2. How to cache queries? → **DAX**
3. Which feature triggers Lambda? → **Streams**

---

## 4. Amazon ElastiCache

### Overview
In-memory caching service supporting Redis and Memcached.

### Key Features
- Redis (persistence, replication, pub/sub)
- Memcached (simple caching)
- Cluster mode for sharding
- Multi-AZ replication

### Architectures
- ElastiCache + RDS for query offloading
- ElastiCache session store for web apps

### Exam Questions
1. Which supports pub/sub messaging? → **Redis**
2. Which is best for simple caching? → **Memcached**
3. Which improves RDS read performance? → **ElastiCache**

---

## 5. Amazon Redshift

### Overview
Redshift is a managed petabyte-scale data warehouse for analytics.

### Key Features
- Columnar storage, compression
- Redshift Spectrum (query S3 directly)
- RA3 nodes with managed storage
- Concurrency Scaling

### Architectures
- Redshift + S3 (ETL/analytics)
- Redshift + QuickSight (BI dashboards)

### Exam Questions
1. Which allows querying data in S3 without loading? → **Redshift Spectrum**
2. Which node type separates compute from storage? → **RA3**
3. Which feature provides temporary additional clusters? → **Concurrency Scaling**

---

## 6. Amazon Neptune

### Overview
Fully managed graph database service optimized for connected data.

### Key Features
- Supports **Gremlin** (Property Graph) and **SPARQL** (RDF)
- Replicas across AZs
- Use cases: fraud detection, recommendation engines

### Architectures
- Neptune + Lambda for fraud detection
- Neptune + S3 for linked data import

### Exam Questions
1. Which query language is supported for RDF? → **SPARQL**
2. Which is best for fraud detection and relationships? → **Neptune**
