# AWS Learning Path - Week by Week 📅

## Overview
This document outlines your 1-week intensive learning path to master AWS fundamentals and prepare for interviews.

---

## 🟢 Week 1: Fundamentals (June 29 - July 5, 2026)

### Day 1-2: Core Concepts & Infrastructure
**Time**: 4-6 hours

**Topics**:
1. What is AWS and Cloud Computing?
2. AWS Global Infrastructure (Regions, AZs)
3. AWS Service Categories
4. Free Tier Overview

**Files to Study**:
- `01-AWS-Fundamentals/01-Core-Concepts.md`
- `01-AWS-Fundamentals/02-Global-Infrastructure.md`

**Key Learning**:
- [ ] Understand what AWS is and its benefits
- [ ] Know all AWS regions and availability zones
- [ ] Understand service categories
- [ ] Set up Free Tier account

---

### Day 3: IAM - The Most Important Concept
**Time**: 3-4 hours

**Topics**:
1. IAM Users, Groups, Roles
2. Policies and Permissions
3. Security Best Practices
4. MFA & Access Keys

**Files to Study**:
- `01-AWS-Fundamentals/03-IAM-Basics.md`

**Key Learning**:
- [ ] Understand IAM architecture
- [ ] Create users and assign permissions
- [ ] Understand the principle of least privilege
- [ ] Know when to use roles vs users

**Hands-On**:
- Create IAM user in AWS Console
- Generate access keys
- Test permissions

---

### Day 4-5: Service Categories Deep Dive
**Time**: 4-5 hours

**Topics**:
1. **Compute Services**: EC2, Lambda, ECS
2. **Storage Services**: S3, EBS, EFS
3. **Database Services**: RDS, DynamoDB
4. **Networking**: VPC, ALB, Route 53
5. **Messaging**: SNS, SQS

**Files to Study**:
- `01-AWS-Fundamentals/04-Service-Categories.md`

**Key Learning**:
- [ ] Know what each service does
- [ ] Understand when to use which service
- [ ] Know the basic architecture
- [ ] Understand pricing models

---

### Day 6: Interview Prep - Start Q&A Review
**Time**: 3-4 hours

**Topics**:
1. Review top 25 basic AWS questions
2. Practice explaining concepts
3. Understand trade-offs

**Files to Study**:
- `03-Interview-Questions/Top-50-AWS-Questions.md` (Part 1)

**Key Learning**:
- [ ] Be able to answer "What is AWS?"
- [ ] Explain EC2 vs Lambda
- [ ] Discuss S3 features
- [ ] Know VPC basics

---

### Day 7: Review & Prepare for Projects
**Time**: 2-3 hours

**Topics**:
1. Review all core concepts
2. Set up AWS CLI
3. Understand AWS Free Tier limits

**Files to Study**:
- `04-AWS-CLI-Commands/CLI-Cheat-Sheet.md`
- AWS Free Tier documentation

**Hands-On**:
- Install AWS CLI
- Configure AWS credentials
- Run basic CLI commands

---

## 🟡 Week 2: Hands-On Projects (July 6 - July 12, 2026)

### Project 1: EC2 & Lambda Basics (2-3 hours)
**Location**: `02-Hands-On-Projects/Project-1-Compute-Basics/`

**What You'll Learn**:
- Launch and manage EC2 instances
- Create and deploy Lambda functions
- Understand compute trade-offs
- Set up security groups

**Interview Relevance**: Compute service selection, scalability, cost optimization

---

### Project 2: S3 & CloudFront (2-3 hours)
**Location**: `02-Hands-On-Projects/Project-2-Storage-S3/`

**What You'll Learn**:
- Upload and manage objects in S3
- Set up versioning and lifecycle policies
- Configure CloudFront CDN
- Implement access controls

**Interview Relevance**: Storage strategies, content delivery, security, cost optimization

---

### Project 3: Database Essentials (3-4 hours)
**Location**: `02-Hands-On-Projects/Project-3-Database-Essentials/`

**What You'll Learn**:
- Create RDS MySQL database
- Set up DynamoDB table
- Understand when to use relational vs NoSQL
- Implement backups and replication

**Interview Relevance**: Database selection, scaling strategies, backup and recovery

---

### Project 4: Serverless Application (3-4 hours)
**Location**: `02-Hands-On-Projects/Project-4-Serverless-App/`

**What You'll Learn**:
- Build API with API Gateway and Lambda
- Use DynamoDB for storage
- Implement authentication
- Monitor with CloudWatch

**Interview Relevance**: Serverless architecture, cost optimization, scalability

---

### Project 5: Full-Stack Multi-Service App (4-5 hours)
**Location**: `02-Hands-On-Projects/Project-5-Full-Stack-Application/`

**What You'll Learn**:
- Combine multiple AWS services
- Design complete architecture
- Implement monitoring and logging
- Discuss trade-offs and optimization

**Interview Relevance**: Architecture design, service integration, best practices

---

## 📚 Interview Preparation Schedule

### Parallel with Projects:
- **Day 1-2**: Review questions 1-20
- **Day 3-4**: Review questions 21-40
- **Day 5-7**: Review questions 41-50 + architecture patterns

### Before Interview:
- Practice explaining each project
- Discuss trade-offs in architecture
- Know best practices for each service
- Understand cost optimization strategies

---

## 🎯 Success Metrics

By the end of Week 2, you should be able to:

✅ **Knowledge**:
- Explain 50 AWS concepts clearly
- Design basic architectures
- Know when to use each service
- Understand pricing and costs

✅ **Skills**:
- Navigate AWS Console confidently
- Use AWS CLI effectively
- Deploy applications on AWS
- Implement security best practices

✅ **Interview Ready**:
- Answer AWS questions comprehensively
- Discuss real-world scenarios
- Explain trade-offs and decisions
- Mention security and best practices

---

## 📖 Daily Study Schedule

### Ideal Daily Schedule:
- **9:00-10:30 AM**: Read 1 concept file (~1.5 hours)
- **10:30-11:00 AM**: Review notes (~30 mins)
- **11:00 AM-1:00 PM**: Hands-on practice (~2 hours)
- **1:00-2:00 PM**: Lunch break
- **2:00-3:00 PM**: AWS Console exploration (~1 hour)
- **3:00-4:00 PM**: Review interview questions (~1 hour)
- **4:00-5:00 PM**: Practice explaining concepts (~1 hour)

**Total Daily**: 6-7 hours

---

## 🛠️ Tools & Resources You'll Need

- ✅ AWS Free Tier Account
- ✅ AWS CLI (installed locally)
- ✅ Text editor (VS Code recommended)
- ✅ Terminal/Command line
- ✅ AWS Management Console access
- ✅ This repository

---

## 💡 Tips for Success

1. **Don't Just Read**: Hands-on is crucial. Do every project.
2. **Understand Concepts**: Don't memorize. Understand why.
3. **Take Notes**: Write down key points and trade-offs.
4. **Practice Explaining**: Say concepts out loud. This helps in interviews.
5. **Monitor Free Tier Usage**: Keep track of what costs money.
6. **Clean Up Resources**: Stop instances and delete resources after projects.
7. **Ask Questions**: If confused, research further. Google is your friend.

---

## ❓ Common Mistakes to Avoid

❌ **Don't**:
- Skip hands-on projects
- Just read without practicing
- Forget to clean up resources (costs money!)
- Ignore security best practices
- Memorize without understanding
- Leave services running after projects

✅ **Do**:
- Follow the learning path sequentially
- Complete all hands-on projects
- Review interview questions daily
- Take breaks (don't burn out)
- Ask for help when stuck
- Delete/stop resources after projects

---

## 📞 Need Help?

Refer to:
- AWS Official Documentation: https://docs.aws.amazon.com/
- AWS Forums: https://forums.aws.amazon.com/
- Stack Overflow (tag: amazon-web-services)
- This repository issues

---

**Good Luck! You've got this! 🚀**

Last updated: June 29, 2026
