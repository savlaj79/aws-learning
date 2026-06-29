# AWS Core Concepts 🌩️

## Table of Contents
1. [What is AWS?](#what-is-aws)
2. [Cloud Computing Models](#cloud-computing-models)
3. [AWS Advantages](#aws-advantages)
4. [AWS vs Traditional IT](#aws-vs-traditional-it)
5. [Key AWS Services Overview](#key-aws-services-overview)

---

## What is AWS?

### Definition
**AWS (Amazon Web Services)** is a comprehensive, evolving cloud computing platform provided by Amazon. It offers a mix of infrastructure-as-a-service (IaaS), platform-as-a-service (PaaS), and packaged software-as-a-service (SaaS) products.

### In Simple Terms
Instead of buying physical servers and managing them yourself, AWS lets you **rent computing resources** on-demand from Amazon's massive data centers worldwide.

### History
- **2006**: AWS launched with S3, SQS, EC2
- **Today**: 200+ services across 30+ regions

---

## Cloud Computing Models

### 1. **IaaS (Infrastructure as a Service)**
You manage: Applications, Data, Runtime, Middleware, OS
AWS manages: Virtualization, Servers, Storage, Networking

**Services**: EC2, VPC, EBS
**Use Case**: Full control, custom applications

### 2. **PaaS (Platform as a Service)**
You manage: Applications, Data
AWS manages: Runtime, Middleware, OS, Virtualization, Servers, Storage, Networking

**Services**: Elastic Beanstalk, App Runner
**Use Case**: Focus on code, not infrastructure

### 3. **SaaS (Software as a Service)**
You use the application
AWS manages: Everything

**Services**: AWS managed services like Amazon Connect
**Use Case**: Use software without managing anything

---

## AWS Advantages

### ✅ Cost Efficiency
- **Pay-as-you-go**: Pay only for what you use
- **No upfront costs**: No capital expenditure
- **Economies of scale**: Lower costs than owning infrastructure

**Example**: 
- Traditional: Buy server ($5,000) + setup + maintenance
- AWS: Pay $0.05/hour only when running

### ✅ Global Reach
- **30+ Regions** worldwide
- **Availability Zones** within each region
- **Deploy anywhere**, serve customers globally
- Low latency for users worldwide

### ✅ Scalability
- **Vertical Scaling**: Add more power to existing resources
- **Horizontal Scaling**: Add more resources
- **Auto-scaling**: Automatically adjust based on demand

**Example**:
- Traffic spike during sale? AWS automatically adds more servers
- Traffic dies down? AWS removes them

### ✅ Reliability & High Availability
- **99.99%** uptime SLA
- **Multiple Availability Zones**
- **Redundancy** built-in
- Disaster recovery options

### ✅ Security
- **Encryption** (data at rest and in transit)
- **IAM** for access control
- **Compliance** certifications (SOC 2, PCI-DSS, HIPAA, etc.)
- AWS responsible for physical security

### ✅ Flexibility
- 200+ services to choose from
- Mix and match as needed
- Supports multiple programming languages
- Works with existing tools

### ✅ Easy to Use
- **AWS Management Console**: Web-based interface
- **AWS CLI**: Command-line interface
- **SDKs**: For programming languages
- Extensive documentation

---

## AWS vs Traditional IT

| Aspect | Traditional IT | AWS |
|--------|---|---|
| **Setup Time** | Months | Minutes |
| **Cost** | High upfront (CapEx) | Pay-as-you-go (OpEx) |
| **Scalability** | Limited, slow | Instant, automatic |
| **Maintenance** | Your responsibility | AWS responsibility |
| **Global Presence** | Expensive to expand | Easy to expand |
| **Uptime Guarantee** | Not guaranteed | 99.99% SLA |
| **Security Updates** | Your job | AWS handles |

---

## Key AWS Services Overview

### Compute Services 🖥️
**What**: Execute code/applications

| Service | What It Does | When to Use |
|---------|--------------|-------------|
| **EC2** | Virtual machines in the cloud | Need full control, custom applications |
| **Lambda** | Run code without managing servers | Short tasks, event-driven |
| **ECS** | Docker container orchestration | Containerized applications |

### Storage Services 💾
**What**: Store data permanently

| Service | What It Does | When to Use |
|---------|--------------|-------------|
| **S3** | Object storage (files, images, videos) | Any file storage need |
| **EBS** | Block storage for EC2 | Persistent storage for servers |
| **EFS** | Shared file system | Shared storage across instances |

### Database Services 🗄️
**What**: Store and retrieve structured data

| Service | What It Does | When to Use |
|---------|--------------|-------------|
| **RDS** | Managed relational database (MySQL, PostgreSQL) | Traditional databases |
| **DynamoDB** | NoSQL database (fast, scalable) | Real-time apps, fast access needed |
| **ElastiCache** | In-memory database (Redis, Memcached) | Caching, sessions, real-time data |

### Networking Services 🌐
**What**: Connect resources and route traffic

| Service | What It Does | When to Use |
|---------|--------------|-------------|
| **VPC** | Virtual private network | Isolated network environment |
| **ALB** | Application Load Balancer | Distribute traffic to multiple servers |
| **Route 53** | DNS service | Domain name management |
| **CloudFront** | Content delivery network (CDN) | Faster content delivery worldwide |

### Messaging & Events 📬
**What**: Communicate between applications

| Service | What It Does | When to Use |
|---------|--------------|-------------|
| **SNS** | Simple Notification Service | Send notifications, alerts |
| **SQS** | Simple Queue Service | Decouple applications, async tasks |
| **EventBridge** | Event routing service | React to events in real-time |

### DevOps & Monitoring 📊
**What**: Deploy, manage, and monitor applications

| Service | What It Does | When to Use |
|---------|--------------|-------------|
| **CloudWatch** | Monitoring and logging | Track metrics, logs, performance |
| **CodePipeline** | CI/CD service | Automate deployment pipeline |
| **CloudFormation** | Infrastructure as code | Define infrastructure in templates |

### Security & Identity 🔐
**What**: Manage access and protect resources

| Service | What It Does | When to Use |
|---------|--------------|-------------|
| **IAM** | Identity and Access Management | Control who can do what |
| **KMS** | Key Management Service | Encryption key management |
| **Secrets Manager** | Store sensitive data | Database passwords, API keys |

---

## AWS Pricing Models

### 1. **On-Demand**
- Pay per hour/second
- No commitment
- Most flexible
- Most expensive

**Best for**: Variable workloads, development/testing

### 2. **Reserved Instances (RI)**
- Commit to 1 or 3 years
- Up to 72% discount
- Locked in

**Best for**: Stable, predictable workloads

### 3. **Spot Instances**
- Bid on spare capacity
- Up to 90% discount
- Can be interrupted

**Best for**: Flexible workloads, batch jobs

### 4. **Savings Plans**
- Commit to hourly spend
- Flexible across services
- Up to 72% discount

**Best for**: Mix of services

---

## AWS Free Tier

### What's Included?
1. **12 months free** for new customers:
   - 750 hours/month EC2 (t2.micro)
   - 5 GB S3 storage
   - 20 GB data transfer

2. **Always free**:
   - AWS Lambda (1 million requests/month)
   - DynamoDB (25 GB storage)
   - CloudWatch (basic monitoring)

3. **Free trials**: 
   - Some services offer 1-month or 12-month trials

**Important**: After free tier, you pay for everything. Monitor your usage!

---

## How AWS Works - High Level Flow

```
1. Sign up for AWS account
        ↓
2. Create IAM users (don't use root account)
        ↓
3. Choose a region (closest to your users)
        ↓
4. Select service (EC2, S3, RDS, etc.)
        ↓
5. Create/configure resources
        ↓
6. Monitor with CloudWatch
        ↓
7. Scale as needed
        ↓
8. Pay based on usage
```

---

## Key Terminology

| Term | Meaning |
|------|---------|
| **Region** | Geographic area where AWS resources are located |
| **Availability Zone (AZ)** | Data center within a region |
| **ARN** | Amazon Resource Name - unique identifier for resources |
| **AMI** | Amazon Machine Image - template for EC2 instances |
| **Subnet** | Segment of VPC network |
| **Security Group** | Virtual firewall for instances |
| **IAM** | Identity and Access Management |
| **Role** | Set of permissions |
| **Policy** | Document defining permissions |

---

## Summary Checklist

- [ ] Understand what AWS is (on-demand computing)
- [ ] Know the 3 cloud models (IaaS, PaaS, SaaS)
- [ ] Know 5 main AWS advantages
- [ ] Familiar with major AWS services
- [ ] Understand AWS pricing models
- [ ] Know what's in Free Tier
- [ ] Understand when to use each service type

---

## Next Steps

1. ✅ Read this document carefully
2. ⬜ Create AWS Free Tier account
3. ⬜ Explore AWS Console
4. ⬜ Move to **02-Global-Infrastructure.md**

---

**Interview Tips**:
- "AWS is Infrastructure as a Service (IaaS) offering computing, storage, networking, and database services"
- Always mention advantages: cost, scalability, global reach, security
- Know the basic services and when to use each one
- Remember: "You pay only for what you use"

---

Last updated: June 29, 2026
