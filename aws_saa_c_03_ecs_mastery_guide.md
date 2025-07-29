## 🐳 Amazon ECS Mastery Guide (SAA-C03 Focus)

This guide simplifies Amazon ECS for the AWS Certified Solutions Architect – Associate (SAA-C03) exam. It includes Fargate, EC2 launch types, networking, IAM roles, logging, storage, secrets, scaling, and deployment strategies.

---

## 🚀 ECS Overview

Amazon ECS (Elastic Container Service) is a highly scalable, fast container management service that supports Docker containers and allows easy deployment and management on AWS.

### 🧱 ECS Components

| Component       | Description                                              |
| --------------- | -------------------------------------------------------- |
| Cluster         | Logical group of EC2/Fargate resources                   |
| Task Definition | Blueprint for running containers (CPU/mem, image, ports) |
| Task            | An instance of a Task Definition running in ECS          |
| Service         | Keeps tasks running; supports ALBs and scaling           |
| Launch Type     | Fargate (serverless) or EC2 (self-managed instances)     |

---

## ☁️ Launch Types: Fargate vs EC2

| Feature      | Fargate                    | EC2                           |
| ------------ | -------------------------- | ----------------------------- |
| Serverless   | ✅                         | ❌ (You manage EC2 instances) |
| Custom AMI   | ❌                         | ✅                            |
| Per-task ENI | ✅ (`awsvpc`)              | ✅ (via `awsvpc`)             |
| Auto Scaling | ✅ With Capacity Providers | ✅ With ASG                   |

---

## 🌐 Networking Modes

| Mode     | Description                                                     |
| -------- | --------------------------------------------------------------- |
| `awsvpc` | Task gets its own ENI, full VPC IP access, required for Fargate |
| `bridge` | Default Docker bridge networking (EC2 only)                     |
| `host`   | Shares host network (EC2 only)                                  |

---

## 🔐 IAM Roles

| Role Type      | Purpose                                                                 |
| -------------- | ----------------------------------------------------------------------- |
| Task Role      | Used by the **containers** to access AWS resources (e.g., S3, DynamoDB) |
| Execution Role | Used by ECS agent to **pull images, publish logs**                      |

---

## 📦 Storage Integration

- **EFS**: Mount persistent file systems to tasks (shared between tasks)
- **Ephemeral Storage**: Default for containers (non-persistent)

---

## 📜 Logging

- **CloudWatch Logs** (default driver)
- **FireLens**: Customizable log routing (e.g., Fluentd, Fluent Bit)

---

## 🔑 Secrets & Env Variables

- Use **AWS Secrets Manager** or **SSM Parameter Store**
- Inject secrets as environment variables or volume mounts

---

## ⚙️ Deployments

| Strategy       | Description                                                        |
| -------------- | ------------------------------------------------------------------ |
| Rolling update | Default ECS behavior; replaces tasks gradually                     |
| Blue/Green     | Via **CodeDeploy**; allows full validation before traffic shifting |

### Deployment Parameters

- `minimumHealthyPercent`: % of tasks that must stay healthy during deploy
- `maximumPercent`: Max number of extra tasks that can run temporarily

---

## 📈 Scaling

- **Service Auto Scaling**: Target tracking based on metrics (CPU, memory, custom CW metrics)
- **Cluster Auto Scaling**: Uses Capacity Providers (launches/destroys EC2 as needed)

---

## 🌐 🔍 Service Discovery (Cloud Map)

Service discovery lets ECS services **automatically register DNS names**, so others can find them even as tasks change IPs.

### 🧭 What is AWS Cloud Map?

- Service registry that maps **DNS names to ECS task IPs**
- Supports `A` records (IP) and `SRV` (IP + port)

### 🧰 How ECS Service Discovery Works

1. Enable discovery when defining ECS service
2. ECS registers task IP in Cloud Map DNS
3. Other services query e.g. `orders.local` to get updated IPs
4. Deregistration happens automatically when tasks stop

### ✅ Best Practices

- Use with `awsvpc` networking (each task gets its own ENI)
- Use in **microservices** where IPs change frequently

### 🧠 Flashcard

| Topic          | Key Point                                 |
| -------------- | ----------------------------------------- |
| AWS Cloud Map  | DNS-based discovery for dynamic ECS tasks |
| ECS + CloudMap | Auto-registration of task IPs             |
| DNS Records    | A (IP), SRV (IP + port)                   |

---

## 🧪 Exam Tip Scenarios

> **Q: Which deployment type in ECS ensures validation before live traffic is shifted?**  
> **A: Blue/Green deployment using CodeDeploy**

> **Q: You need per-task isolation, IAM, and secure networking. Which mode?**  
> **A: Fargate + `awsvpc` networking**

> **Q: Which IAM role is used by containers to access AWS services like S3 or DynamoDB?**  
> **A: Task Role**

> **Q: Your ECS service needs to allow other microservices to resolve its IP dynamically. What do you use?**  
> **A: AWS Cloud Map with Service Discovery enabled**

> **Q: You want to mount a persistent volume that is shared across multiple ECS tasks. What should you use?**  
> **A: Amazon EFS volume mounted in the task definition**

> **Q: Where do ECS logs go by default?**  
> **A: Amazon CloudWatch Logs**

> **Q: How do you control how many tasks are replaced during an ECS rolling update?**  
> **A: Use `minimumHealthyPercent` and `maximumPercent` deployment parameters**

> **Q: You need fine-grained scaling based on ECS service CPU usage. What should you use?**  
> **A: ECS Service Auto Scaling with target tracking policies**

> **Q: How does ECS EC2 mode scale out the underlying infrastructure?**  
> **A: With Cluster Auto Scaling using Capacity Providers**

> **Q: Which deployment type in ECS ensures validation before live traffic is shifted?**  
> **A: Blue/Green deployment using CodeDeploy**

> **Q: You need per-task isolation, IAM, and secure networking. Which mode?**  
> **A: Fargate + `awsvpc` networking**

---
