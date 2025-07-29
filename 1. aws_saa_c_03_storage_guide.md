**AWS SAA-C03 – Storage Mastery Guide**

---

## 📄 Section 1: AWS Storage Services Cheat Sheet

### 1. Amazon S3 (Simple Storage Service)

- **Type**: Object storage
- **Durability**: 11 9s
- **Access control**: IAM, Bucket Policy, ACL
- **Features**: Versioning, Lifecycle Rules, Encryption (SSE-S3, SSE-KMS)
- **Storage Classes**: Standard, Intelligent-Tiering, Standard-IA, One Zone-IA, Glacier (Instant/Flexible/Deep Archive)
- **Use Case**: Static websites, backups, logs, data lakes

### 2. Amazon EBS (Elastic Block Store)

- **Type**: Block storage for EC2
- **AZ Scope**: Single AZ
- **Types**: gp3 (general), io2 (high IOPS), st1, sc1
- **Features**: Snapshots (incremental, stored in S3), encryption, resize
- **Use Case**: Databases, boot volumes, persistent EC2 storage

### 3. Amazon EFS (Elastic File System)

- **Type**: Shared file system (Linux)
- **Scope**: Multi-AZ (regional)
- **Features**: NFS, scales automatically, lifecycle to IA, bursting/provisioned throughput
- **Use Case**: Shared Linux storage, web servers

### 4. Amazon FSx

- **FSx for Windows**: SMB protocol, AD integration
- **FSx for Lustre**: High-performance, integrates with S3
- **Use Case**: Windows apps, HPC workloads

### 5. AWS Storage Gateway

- **Types**:
  - File Gateway: NFS/SMB interface to S3
  - Volume Gateway: Block storage with cloud backup
  - Tape Gateway: Virtual tape library
- **Use Case**: Hybrid cloud, backup, DR

### 6. Amazon S3 Glacier

- **Cold storage classes**:
  - Glacier Instant Retrieval
  - Glacier Flexible Retrieval
  - Glacier Deep Archive
- **Use Case**: Archival, compliance, infrequent access

---

## 📊 Section 2: Architecture Diagrams

### 1. Static Website Hosting with CloudFront

```
User --> CloudFront (with OAI) --> S3 Bucket (public access blocked)
```

### 2. Shared Linux File System

```
          +-----------+
EC2 (AZ A)|           |
          |   EFS     | <-- Shared, scalable
EC2 (AZ B)|           |
          +-----------+
```

### 3. Hybrid Cloud Backup

```
On-Premises App --> Storage Gateway --> S3 / Glacier
```

---

## 🔮 Section 3: Real Exam-Style Questions

### Basic Questions

1. **Best service for static asset hosting?**

   - A: EBS  B: S3 ✅  C: EFS  D: FSx

2. **Cost-effective cold archive?**

   - A: IA  B: Glacier Instant  C: Glacier Deep Archive ✅

3. **Shared file system for Linux EC2?**

   - A: FSx  B: EBS  C: S3  D: EFS ✅

4. **Backup virtual tape infra?**

   - A: DataSync  B: Snowball  C: Storage Gateway - Tape ✅

5. **Cross-region replication of objects?**

   - A: Versioning  B: CRR ✅  C: Lifecycle Rule  D: S3 Select

### Advanced Architecture Questions

6. **Serverless data lake**

   - A: RDS + Lambda  B: S3 + Athena ✅  C: FSx + CloudWatch  D: Redshift

7. **Multi-AZ shared access for EC2 Linux?**

   - A: FSx for Windows  B: EFS ✅  C: S3  D: EBS

8. **Secure website access from S3?**

   - A: S3 + CloudFront + OAI ✅

9. **Hybrid volume cache backup?**

   - A: FSx  B: Storage Gateway Volume Gateway ✅

10. **App availability in region failure?**

- A: S3-IA  B: Versioning  C: CRR ✅  D: EFS

---

## 🔄 Section 4: Flashcards (Memory Boost)

| **Question**                          | **Answer**           |
| ------------------------------------- | -------------------- |
| Object storage service in AWS?        | S3                   |
| File storage for Linux EC2s?          | EFS                  |
| Block-level storage for EC2?          | EBS                  |
| Archive storage for compliance?       | Glacier Deep Archive |
| Prevent deletion of S3 objects?       | Object Lock          |
| Hybrid access to S3 for SMB?          | File Gateway         |
| High IOPS EBS type?                   | io2                  |
| NFS-based, scalable file system?      | EFS                  |
| S3 access without making public?      | CloudFront + OAI     |
| Auto-move objects to cheaper storage? | Lifecycle Policies   |

---

## ⚡ Exam Tips

- **Use Intelligent-Tiering** for unpredictable access
- **S3 + CloudFront + OAI** is secure static hosting
- **Glacier Deep Archive** is the cheapest archive class
- **Storage Gateway** bridges on-prem and AWS
- Use **Snapshots** to back up EBS or share AMIs
- Know EBS volume types: gp3 (default), io2 (high IOPS)
- FSx for Windows = SMB + AD; FSx for Lustre = HPC, speed
- Understand how **S3 integrates with Athena, Lambda, CloudFront**

---

End of Document

