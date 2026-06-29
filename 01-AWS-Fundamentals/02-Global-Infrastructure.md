# AWS Global Infrastructure 🌍

## Table of Contents
1. [Infrastructure Overview](#infrastructure-overview)
2. [Regions](#regions)
3. [Availability Zones](#availability-zones)
4. [Edge Locations](#edge-locations)
5. [Choosing a Region](#choosing-a-region)
6. [Architecture Diagrams](#architecture-diagrams)

---

## Infrastructure Overview

AWS global infrastructure is built on three concepts:

```
AWS Global Infrastructure
    │
    ├─ Regions (30+)
    │  └─ Availability Zones (2+ per region)
    │     └─ Data Centers
    │
    ├─ Edge Locations (400+)
    │  └─ For CloudFront & Route 53
    │
    └─ Local Zones
       └─ Extended AWS infrastructure
```

### Why This Design?
- ✅ **High Availability**: If one AZ fails, others still work
- ✅ **Low Latency**: Serve users from nearby locations
- ✅ **Disaster Recovery**: Multiple geographic locations
- ✅ **Compliance**: Data residency requirements

---

## Regions

### What is a Region?
A **Region** is a **geographic area** that contains multiple Availability Zones. Each region is isolated and independent.

### Current AWS Regions (30+)

#### North America
- **us-east-1** (N. Virginia) - Oldest region, most services
- **us-west-1** (N. California)
- **us-west-2** (Oregon)
- **ca-central-1** (Canada)

#### Europe
- **eu-west-1** (Ireland) - Most popular in Europe
- **eu-central-1** (Frankfurt)
- **eu-north-1** (Stockholm)
- **eu-west-2** (London)
- **eu-west-3** (Paris)

#### Asia Pacific
- **ap-southeast-1** (Singapore)
- **ap-southeast-2** (Sydney)
- **ap-northeast-1** (Tokyo)
- **ap-south-1** (Mumbai)

#### South America
- **sa-east-1** (São Paulo)

#### Middle East
- **me-south-1** (Bahrain)

#### Africa
- **af-south-1** (Cape Town)

### Region Characteristics

| Aspect | Details |
|--------|---------|
| **Services** | Most regions have all services, some new ones deploy to us-east-1 first |
| **Cost** | Varies by region (us-east-1 is usually cheapest) |
| **Latency** | Choose region closest to users |
| **Compliance** | Some data must stay in specific regions |
| **Independence** | Each region is completely isolated |

---

## Availability Zones (AZs)

### What is an Availability Zone?
An **Availability Zone** is one or more **data centers** within a region. Each AZ is:
- Physically separate (different buildings)
- Independently powered
- Independently cooled
- Independently networked
- Connected by low-latency, high-bandwidth links

### Why Multiple AZs?
```
Multiple AZ Architecture:

    Region (e.g., us-east-1)
    │
    ├─ AZ-1 (us-east-1a)
    │  ├─ Data Center 1
    │  ├─ EC2 Instances
    │  └─ Database Replica
    │
    ├─ AZ-2 (us-east-1b)
    │  ├─ Data Center 2
    │  ├─ EC2 Instances
    │  └─ Database Replica
    │
    └─ AZ-3 (us-east-1c)
       ├─ Data Center 3
       ├─ EC2 Instances
       └─ Database Replica

If AZ-1 fails → Traffic automatically goes to AZ-2 and AZ-3
```

### AZ Features

| Feature | Details |
|---------|---------|
| **Count** | Most regions have 2-3 AZs |
| **Naming** | region-code + letter (e.g., us-east-1a) |
| **Connection** | Connected via dedicated metro fiber |
| **Latency** | < 1ms between AZs |
| **Failure** | Failure in one AZ doesn't affect others |

### High Availability Architecture Example

```
Without HA (Single AZ):
┌─────────────────────────────────┐
│ Availability Zone 1             │
│  ┌───────────────────────────┐  │
│  │ EC2 Instance              │  │
│  │ Database                  │  │
│  │ Application               │  │
│  └───────────────────────────┘  │
│                                 │
│ ⚠️ AZ fails → Everything down!  │
└─────────────────────────────────┘

With HA (Multi-AZ):
┌─────────────────┐  ┌─────────────────┐
│ Availability    │  │ Availability    │
│ Zone 1          │  │ Zone 2          │
│ ┌─────────────┐ │  │ ┌─────────────┐ │
│ │ EC2 Server  │─┼──┼─│ EC2 Server  │ │
│ │ Load Balancer (ALB)       │ │
│ └─────────────┘ │  │ └─────────────┘ │
│ ┌─────────────┐ │  │ ┌─────────────┐ │
│ │ Database    │─┼──┼─│ Database    │ │
│ │ (Primary)   │ │  │ │ (Standby)   │ │
│ └─────────────┘ │  │ └─────────────┘ │
└─────────────────┘  └─────────────────┘
  ✅ AZ-1 fails → Traffic goes to AZ-2
```

---

## Edge Locations

### What are Edge Locations?
**Edge Locations** are endpoints where AWS caches content near end users. Used for:
- CloudFront (CDN)
- Route 53 (DNS)
- AWS Shield (DDoS protection)

### How Many?
- **400+** Edge Locations worldwide
- More than Regions & AZs
- Strategically placed near cities

### Edge Location vs Region
| Aspect | Region | Edge Location |
|--------|--------|---------------|
| **Purpose** | Host resources | Cache content |
| **Services** | All major services | CloudFront, Route 53 |
| **Count** | 30+ | 400+ |
| **Data Storage** | Full applications | Cached copies only |

### Edge Location Benefits

```
Without CloudFront (Edge Locations):
User in Singapore
        │
        │ (9000+ km, slow)
        ↓
AWS Region in us-east-1
        ↓ (fetch response)
        │ (9000+ km, slow)
        ↓
User ⚠️ High latency!

With CloudFront (Edge Locations):
User in Singapore
        │
        │ (100 km, fast)
        ↓
Edge Location in Singapore (cached content)
        ↓ (if not cached)
        │
        ↓
AWS Region in us-east-1
        ↓
User ✅ Low latency!
```

---

## Choosing a Region

### Factors to Consider

#### 1. **Latency** ⏱️
- Choose region closest to your users
- Lower latency = better user experience

#### 2. **Compliance & Data Residency** 📋
- Some regulations require data to stay in specific countries
- GDPR (Europe), HIPAA (Healthcare), etc.

#### 3. **Service Availability** 🔧
- New services launch in us-east-1 first
- Some specialized services only in specific regions

#### 4. **Cost** 💰
- us-east-1 is usually cheapest
- Pricing varies by region
- 10-20% more expensive in some regions

#### 5. **AWS Support** 🤝
- Business hours support available in different timezones

### Decision Matrix

```
Which region should I choose?

My primary users are in:
├─ North America → us-east-1 or us-west-2
├─ Europe → eu-west-1 (Ireland)
├─ Asia-Pacific → ap-southeast-1 (Singapore) or ap-northeast-1 (Tokyo)
├─ Australia → ap-southeast-2 (Sydney)
└─ Brazil → sa-east-1 (São Paulo)

Data must stay in specific country?
├─ Yes → Choose that country's region
└─ No → Continue to latency check

Multiple regions needed?
├─ Yes → Primary + backup region
└─ No → Single region

Cost sensitive?
├─ Yes → us-east-1 (cheapest)
└─ No → User latency wins
```

---

## Multi-Region Architecture

### Why Multi-Region?
- 🌍 **Global presence** - Serve users everywhere
- 🔒 **Disaster recovery** - If one region fails
- 📊 **Compliance** - Data in multiple countries
- ⚡ **Performance** - Ultra-low latency

### Common Multi-Region Patterns

#### 1. Active-Passive (Backup Region)
```
Primary Region (Active)
    ↓
    ├─ Serve all users
    ├─ Write data
    └─ Replicate to backup

Backup Region (Passive)
    ↓
    ├─ Standby
    ├─ Read-only replicas
    └─ Activated if primary fails

Use Case: Mission-critical apps, disaster recovery
```

#### 2. Active-Active (Load Distribution)
```
Region 1 (Active)
    ↓
    ├─ Route 53 (Global DNS)
    ├─ Users from Asia
    └─ Serve 50% traffic

Region 2 (Active)
    ↓
    ├─ Route 53 (Global DNS)
    ├─ Users from US
    └─ Serve 50% traffic

Use Case: Global apps, geo-distributed users
```

---

## Architecture Diagrams

### Single Region, Multi-AZ (Most Common)
```
┌──────────────── us-east-1 Region ─────────────────┐
│                                                    │
│  Route 53 (DNS)                                   │
│        │                                           │
│        ├─ ALB (Load Balancer)                     │
│        │                                           │
│  ┌─────┴──────────────────────────────────┐       │
│  │                                        │       │
│  ↓                                        ↓       │
│ us-east-1a (AZ)                us-east-1b (AZ)  │
│ ┌──────────────────┐           ┌──────────────────┐
│ │ EC2 Web Server   │           │ EC2 Web Server   │
│ │ Application      │           │ Application      │
│ └──────────────────┘           └──────────────────┘
│         │                              │           │
│         └──────────────┬───────────────┘           │
│                        │                           │
│              ┌─────────┴────────────┐              │
│              │                      │              │
│  ┌───────────▼──────┐    ┌──────────▼────────────┐
│  │ RDS (Primary)    │    │ RDS (Standby)        │
│  │ MySQL Database   │    │ Auto Failover        │
│  └──────────────────┘    └──────────────────────┘
│                                                    │
└────────────────────────────────────────────────────┘
```

### Multi-Region Active-Active
```
┌─────────────────────────────────────────────────────────────────┐
│                    Route 53 (Global DNS)                        │
│                   (Geo-location routing)                        │
│                                                                 │
│  ┌──────────────────────────┬──────────────────────────┐       │
│  │                          │                          │       │
│  ↓                          ↓                          ↓       │
│  us-east-1                eu-west-1                ap-northeast-1
│  (Primary)                (Secondary)              (Tertiary)
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Web Application + Database                             │  │
│  │ Replicate data between regions                         │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

Users in Asia → routed to ap-northeast-1
Users in Europe → routed to eu-west-1
Users in North America → routed to us-east-1
```

---

## AWS Global Accelerator

### What is it?
Service that optimizes global routing using AWS's private network instead of public internet.

### Benefits
- 🚀 Faster performance (10-60% improvement)
- 🔒 Better security
- 📊 More reliable

---

## Summary Checklist

- [ ] Understand Regions (geographic areas)
- [ ] Understand Availability Zones (data centers within regions)
- [ ] Know difference between Regions and AZs
- [ ] Understand Edge Locations and CloudFront
- [ ] Know how to choose a region (latency, compliance, cost)
- [ ] Familiar with Multi-AZ architecture
- [ ] Know benefits of Multi-Region setup

---

## Key Terminology Review

| Term | Definition |
|------|-----------|
| **Region** | Geographic area with multiple AZs |
| **Availability Zone** | One or more data centers in a region |
| **Edge Location** | Content caching endpoint (400+) |
| **Data Center** | Physical facility with computers |
| **High Availability** | System stays up even if parts fail |
| **Disaster Recovery** | Plan to recover from failures |
| **Multi-AZ** | Application in multiple AZs within region |
| **Multi-Region** | Application in multiple geographic regions |

---

## Interview Questions

**Q1: What's the difference between a Region and an Availability Zone?**
A: A Region is a geographic area (e.g., us-east-1), and an Availability Zone is a data center within that region (e.g., us-east-1a). Multiple AZs in a region are isolated from each other.

**Q2: How many Availability Zones are in a Region?**
A: Most regions have 2-3 AZs, connected by low-latency links.

**Q3: What are Edge Locations used for?**
A: Edge Locations are used by CloudFront (CDN) and Route 53 to cache content and serve it from locations near users.

**Q4: How do you choose a Region?**
A: Consider latency (choose nearest region), compliance (data residency), service availability, and cost.

**Q5: What's the advantage of Multi-AZ deployment?**
A: High availability - if one AZ fails, your application continues running in other AZs.

---

## Next Steps

1. ✅ Understand AWS Global Infrastructure
2. ⬜ Explore AWS Console to see Regions
3. ⬜ Move to **03-IAM-Basics.md**

---

Last updated: June 29, 2026
