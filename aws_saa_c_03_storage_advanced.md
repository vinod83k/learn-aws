**AWS SAA-C03 – Advanced Storage Security & Hybrid Architecture Guide**

---

## 🔐 Section 1: Storage Security Deep Dive

### 1. S3 + KMS Encryption Scenarios

#### a. SSE-S3 (Server-Side Encryption with Amazon S3-managed keys)

- Encryption is handled automatically by S3 using AES-256.
- No need to manage keys or permissions.
- Simplest form of encryption, but lacks fine-grained control or audit trail.
- Suitable for non-sensitive data where encryption is a checkbox requirement.

#### b. SSE-KMS (Server-Side Encryption with AWS KMS - Recommended for exam)

- AWS manages encryption operations, but you control access to keys.
- Use IAM policies to define who can use which keys.
- Enables audit logging through AWS CloudTrail for compliance tracking.
- You can create, rotate, disable, or revoke customer-managed CMKs.
- Common in regulated industries such as healthcare, finance, and government.

#### Example: S3 Bucket Policy to Allow Access with SSE-KMS

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-bucket/*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-server-side-encryption": "aws:kms"
        }
      }
    }
  ]
}
```

> This policy ensures that all uploads are encrypted using KMS keys and denies uploads without encryption.

#### c. Client-Side Encryption

- Encryption occurs before data is sent to AWS.
- Offers the highest level of control but is operationally complex.
- Requires key management on the client side.
- Typically used when compliance dictates that AWS should never see the raw data.

---

### 2. S3 Access Control Scenarios

| Scenario                                  | Solution                                                                                      |
| ----------------------------------------- | --------------------------------------------------------------------------------------------- |
| Only allow objects encrypted with SSE-KMS | Use a bucket policy with the condition `"s3:x-amz-server-side-encryption": "aws:kms"`         |
| Prevent public access to all buckets      | Use Account-Level Block Public Access (BPA) settings to override individual bucket settings   |
| Allow temporary upload access             | Generate pre-signed URLs that expire after a defined duration (e.g., 1 hour)                  |
| Share S3 access within private network    | Use a VPC Gateway Endpoint with IAM and bucket policies that restrict access to specific VPCs |
| Audit all access to S3 buckets            | Enable AWS CloudTrail for API activity and S3 access logs for object-level tracking           |

---

## 🏛️ Section 2: Hybrid Cloud Storage Scenarios

### 1. AWS Storage Gateway Use Cases

#### a. File Gateway

- Ideal for backup and archive use cases.
- Allows NFS or SMB mount on on-premises servers.
- Files appear as local but are backed by S3.
- Local cache ensures frequently accessed files load quickly.
- Used in environments needing seamless file sharing without full cloud migration.

#### b. Volume Gateway

- Presents iSCSI (Internet Small Computer Systems Interface) block devices to on-premises applications by using TCP/IP-based networking. This allows storage volumes created in AWS to appear as locally attached drives on on-prem servers. The technology encapsulates SCSI commands within IP packets, enabling efficient block-level data access over standard Ethernet networks, which is commonly used in Storage Area Network (SAN) implementations.
- Cached volumes: Keep frequently accessed data locally; store full volume in AWS.
- Stored volumes: Keep full volume locally; back up snapshots to AWS.
- Supports point-in-time snapshots for disaster recovery (DR), allowing you to capture the state of a volume at a specific moment. These snapshots can be scheduled or triggered manually, and are stored securely in Amazon S3. In the event of data corruption, accidental deletion, or system failure, you can use these snapshots to restore the volume to a previous known-good state, minimizing data loss and enabling faster recovery. Snapshots are incremental and efficient, capturing only the changes since the last snapshot.
- Used for lift-and-shift scenarios where latency is critical.

#### c. Tape Gateway

- Emulates virtual tape libraries.
- Compatible with existing backup software.
- Virtual tapes are stored in S3 and automatically archived to Glacier.
- Great for customers with existing tape-based workflows seeking modernization.

### 2. Security for Storage Gateway

- Uses KMS for encryption of data at rest.
- IAM roles control what actions the gateway can perform in your account.
- Configure alarms in CloudWatch to track gateway status and usage.
- Audit gateway activity using CloudTrail.

---

## 🔄 Section 3: Architecture Diagram Recognition Tips

### Common Icons and Patterns

| Component            | What to Look For                                                         |
| -------------------- | ------------------------------------------------------------------------ |
| S3 + CloudFront      | CloudFront icon in front of a private S3 bucket with OAI enforced        |
| EC2 + EBS + KMS      | EC2 instance using encrypted EBS volume with KMS integration             |
| Lambda + S3 Trigger  | Event-based architecture (Lambda icon + S3 bucket + trigger arrow)       |
| Storage Gateway + S3 | On-prem storage with cloud icon labeled Storage Gateway or hybrid symbol |
| VPC Endpoint to S3   | VPC with a gateway icon linked to S3 without public access arrows        |

### Sample: Private File Upload

```
User --> API Gateway --> Lambda --> S3 (SSE-KMS) --> CloudTrail Logs
```

> Serverless and secure architecture ensuring all data is encrypted and auditable.

### Sample: Hybrid Backup Architecture

```
On-Prem Backup Server --> Storage Gateway (File) --> S3 --> Glacier Deep Archive
```

> Often labeled with “low cost,” “archival,” or “virtual tapes.”

---

## 🌟 Section 4: Real-World Case Study Questions

### Q1. Secure Archival

**Scenario**: A healthcare provider needs to store medical records for 10 years, rarely accessed but needs full encryption and auditability.

**Recommended Solution**:

- Enable Object Lock in **Compliance mode** for S3 bucket
- Use **SSE-KMS** for encryption with access controls and logs
- Apply lifecycle policies to move data to **Glacier Deep Archive** after 30 days

### Q2. On-Prem File Share Migration

**Scenario**: A financial company has legacy file shares used by dozens of employees. Migration must be seamless.

**Recommended Solution**:

- Deploy **AWS Storage Gateway - File Gateway**
- Use SMB interface to allow Windows access
- Store backend files in Amazon S3, with optional local caching

### Q3. Restricting Unencrypted Uploads

**Scenario**: Security policies require that all uploaded files must be encrypted with KMS.

**Recommended Solution**:

- Implement a **bucket policy** that denies `s3:PutObject` unless `x-amz-server-side-encryption` is set to `aws:kms`
- Educate users and applications to include correct encryption headers

### Q4. Fast Recovery from Local Failure

**Scenario**: A manufacturing company wants to back up its on-prem VM snapshots and recover quickly in case of hardware failure.

**Recommended Solution**:

- Deploy **AWS Storage Gateway - Volume Gateway** in **Cached Mode**
- Snapshots stored in S3; frequently accessed blocks cached locally
- Enable scheduled snapshots for point-in-time recovery

