# AWS ECR Guide for AWS SAA-C03 Exam (Architect Perspective)

## What is Amazon Elastic Container Registry (ECR)?
Amazon Elastic Container Registry (Amazon ECR) is a fully managed Docker container registry that makes it easy to store, manage, and deploy Docker container images. It is **secure**, **scalable**, and **reliable**. ECR tightly integrates with AWS Identity and Access Management (IAM), ECS, EKS, and your CI/CD pipelines.[7][10]

## Key Features (with Exam Relevance)
- **Private & public repositories:** Store Docker images accessible by your workloads or publicly available for sharing.
- **Lifecycle policies:** Automate image cleanup, save storage costs, enforce immutability.[7]
- **Image scanning:** Identify vulnerabilities as part of your secure image pipeline (scan on push or on demand).[7]
- **Cross-region/account replication:** Maintain global registry copies for disaster recovery and multi-region architecture.[7][10]
- **Pull through cache:** Keep frequently used images updated locally by caching them from upstream registries.[7]
- **Integration with ECS & EKS:** Automates image pulls for containerized workloads. ECR is referenced directly in Task Definitions for seamless deployments.[10]
- **Access control with IAM:** Secure image access using IAM policies and resource-based policies.[10]
- **Encryption:** Images are encrypted at rest (Amazon S3, KMS) and in transit (HTTPS).[10]
- **High durability:** Uses S3 under the hood, providing multi-AZ durability.[10]

## AWS Architectural Use Cases
- **Microservices workflows:** Store container images for microservices deployed on ECS/EKS/Fargate.
- **Hybrid Cloud:** Use ECR with on-prem and cloud workloads for a consistent container registry.
- **Multi-region deployments:** Use replication for disaster recovery or low-latency image pulls.
- **DevSecOps:** Integrate scanning into CI/CD for security compliance.

## AWS ECR Concepts for the SAA-C03 Exam
- **Resource Policies:** ECR can use resource-based policies in addition to IAM for repo-level permissions.
- **Lifecycle Management:** Set up automated image deletion for cost-effective storage management.
- **Registry vs. Repository:** Registry is the entire ECR service instance, repository is a specific image collection/group.
- **Scan on Push:** Security best practice for identifying vulnerabilities before deployment.
- **Replication:** Enables sharing of approved images across teams/accounts/regions.
- **Performance:** Pull Through Cache rules keep image pulls fast by reducing external dependencies.

## Realistic Exam Scenarios (Exam-Style Questions)

### Q1: Security & Access Control
_A company requires all container images built by the DevOps team to be scanned for vulnerabilities automatically and only certain IAM roles must be able to push and pull images. Which approach ensures security and compliance?_
- A) Enable scan on push in ECR repos, set up resource-based policies for required IAM roles, and ensure only signed images are allowed.
- B) Enable versioning on S3 buckets, add bucket policies, and require encryption keys for access.
- C) Allow public access to ECR, use KMS for image scans.
- **Correct:** A

### Q2: Cross-Region Architecture
_You need to deploy a microservices application across multiple AWS regions for disaster recovery. How to ensure that the latest application images are always available in all regions with minimal operational overhead?_
- A) Use manual Docker push to all region-specific repositories.
- B) Enable ECR repository replication across regions for automated sync.
- C) Use S3 sync to copy images to different regions and pull from S3.
- **Correct:** B

### Q3: Cost Optimization
_Which feature is used to automatically delete old, unused container images and reduce ECR storage costs?_
- A) Image Replication
- B) Lifecycle Policies
- C) Versioning
- **Correct:** B

### Q4: Integration with CI/CD Pipelines
_A developer needs fully automated security checks on all pushed images as part of the release process. Which ECR setting and AWS service pairing is optimal?_
- A) Enable scan-on-push in ECR, integrate with AWS CodePipeline for orchestration.
- B) Use EC2 to run Docker scans, store results in DynamoDB.
- C) Use CloudFormation StackSets to trigger scans.
- **Correct:** A

### Q5: Access Control Between Accounts
_AWS organization requires Account A to share specific trusted container images with Account B securely. How should this be architected?_ 
- A) Use ECR cross-account replication (private registry replication) and resource-based IAM policies.
- B) Upload images to S3 and share S3 bucket access.
- C) Use CLoudFront to cache ECR images.
- **Correct:** A


## Reference Links (Direct AWS Docs)
- What is ECR? ([docs.aws.amazon.com](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html))
- ECR Features ([aws.amazon.com](https://aws.amazon.com/ecr/features/))
- IAM & Access Control for ECR ([docs.aws.amazon.com](https://docs.aws.amazon.com/AmazonECR/latest/userguide/access-control.html))
- ECR Lifecycle Policies ([docs.aws.amazon.com](https://docs.aws.amazon.com/AmazonECR/latest/userguide/LifecyclePolicies.html))
- ECR Pricing ([aws.amazon.com](https://aws.amazon.com/ecr/pricing/))
