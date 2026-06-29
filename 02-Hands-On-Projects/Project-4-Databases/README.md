# Project 4: Databases - RDS & DynamoDB 🗄️

## Overview
Create relational and NoSQL databases, connect applications, and manage data.

**Time**: 2-3 hours
**Cost**: FREE (within Free Tier)
**Difficulty**: 🟡 Intermediate

---

## What You'll Learn
- [ ] Create RDS MySQL database
- [ ] Create DynamoDB NoSQL database
- [ ] Connect and query databases
- [ ] Understand when to use each
- [ ] Implement backups
- [ ] Monitor database performance

---

## RDS (Relational Database Service)

### What is RDS?
Managed relational database service. AWS handles backups, patches, maintenance.

```
Traditional Database:
├── You manage server
├── You install database
├── You handle backups
├── You manage patches
└── You optimize performance

RDS:
├── AWS manages server
├── AWS installs database
├── AWS handles backups
├── AWS manages patches
└── AWS optimizes hardware
```

### Project 1: Create RDS MySQL Database

#### Step 1: Create Database

1. **Go to AWS Console** → Search "RDS"
2. **Click "Create database"**
3. **Configure**:
   ```
   Engine: MySQL
   Version: 8.0.latest
   Instance class: db.t3.micro (Free Tier)
   Storage: 20 GB (Free Tier)
   DB instance identifier: mylearningdb
   Username: admin
   Password: YourSecurePassword123!
   ```
4. **Create database** (takes 5-10 minutes)

#### Step 2: Configure Security Group

1. **In RDS Console**
2. **Modify database**
3. **VPC security group** → Create new or modify existing
4. **Inbound rules**:
   ```
   Type: MySQL/Aurora
   Port: 3306
   Source: 0.0.0.0/0 (for learning only, not production!)
   ```
5. **Apply immediately**

#### Step 3: Connect to Database

**Install MySQL client**:
```bash
# Mac
brew install mysql-client

# Ubuntu/Linux
sudo apt-get install mysql-client-core

# Windows
# Download from https://dev.mysql.com/downloads/mysql/
```

**Connect**:
```bash
mysql -h mylearningdb.123456.us-east-1.rds.amazonaws.com \
      -u admin \
      -p
# Enter password
```

#### Step 4: Create Tables & Add Data

```sql
-- Create database
CREATE DATABASE myapp;
USE myapp;

-- Create table
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert data
INSERT INTO users (name, email) VALUES
('John Doe', 'john@example.com'),
('Jane Smith', 'jane@example.com'),
('Bob Johnson', 'bob@example.com');

-- Query data
SELECT * FROM users;

-- Update
UPDATE users SET email = 'newemail@example.com' WHERE id = 1;

-- Delete
DELETE FROM users WHERE id = 3;
```

---

## DynamoDB (NoSQL Database)

### What is DynamoDB?
Fully managed NoSQL database. Fast, scalable, serverless.

```
Relational (RDS):
├── Structured tables with rows/columns
├── SQL queries
├── ACID transactions
└── Use: Apps with fixed schema

NoSQL (DynamoDB):
├── Flexible document/item storage
├── Key-Value access
├── Eventually consistent
└── Use: Real-time apps, flexible schema
```

### Project 2: Create DynamoDB Table

#### Step 1: Create Table

1. **Go to AWS Console** → Search "DynamoDB"
2. **Click "Create table"**
3. **Configure**:
   ```
   Table name: Users
   Partition key: user_id (String)
   Sort key: (optional, skip for now)
   Billing mode: On-demand (Pay per request)
   ```
4. **Create table**

#### Step 2: Add Items

1. **Select table**
2. **Click "Explore items"**
3. **Create item**:

```json
{
  "user_id": "user123",
  "name": "John Doe",
  "email": "john@example.com",
  "age": 30,
  "city": "New York"
}
```

4. **Add multiple items**

#### Step 3: Query Items

```python
import boto3
from boto3.dynamodb.conditions import Key

# Connect to DynamoDB
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Users')

# Get item
response = table.get_item(
    Key={'user_id': 'user123'}
)
print(response['Item'])

# Put item
table.put_item(
    Item={
        'user_id': 'user456',
        'name': 'Jane Smith',
        'email': 'jane@example.com'
    }
)

# Scan (get all items)
response = table.scan()
for item in response['Items']:
    print(item)

# Update item
table.update_item(
    Key={'user_id': 'user123'},
    UpdateExpression='SET #n = :val',
    ExpressionAttributeNames={'#n': 'name'},
    ExpressionAttributeValues={':val': 'John Updated'}
)

# Delete item
table.delete_item(
    Key={'user_id': 'user456'}
)
```

---

## RDS vs DynamoDB

| Aspect | RDS | DynamoDB |
|--------|-----|----------|
| **Type** | Relational SQL | NoSQL Key-Value |
| **Schema** | Fixed | Flexible |
| **Query** | SQL | Key/Scan |
| **Scaling** | Manual | Automatic |
| **Consistency** | Strong | Eventually consistent |
| **Use Case** | Traditional apps | Real-time, flexible |
| **Cost** | Pay for instance | Pay per request |

---

## AWS CLI Commands

### RDS

```bash
# Create database
aws rds create-db-instance \
  --db-instance-identifier mydb \
  --db-instance-class db.t3.micro \
  --engine mysql \
  --master-username admin \
  --master-user-password password123

# List databases
aws rds describe-db-instances

# Delete database
aws rds delete-db-instance \
  --db-instance-identifier mydb \
  --skip-final-snapshot
```

### DynamoDB

```bash
# Create table
aws dynamodb create-table \
  --table-name Users \
  --attribute-definitions AttributeName=user_id,AttributeType=S \
  --key-schema AttributeName=user_id,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST

# Put item
aws dynamodb put-item \
  --table-name Users \
  --item '{"user_id": {"S": "user123"}, "name": {"S": "John"}}'

# Get item
aws dynamodb get-item \
  --table-name Users \
  --key '{"user_id": {"S": "user123"}}'

# Scan table
aws dynamodb scan --table-name Users

# Delete table
aws dynamodb delete-table --table-name Users
```

---

## Database Pricing

```
RDS Free Tier (per month):
├── 750 hours of db.t3.micro
├── 20 GB storage
└── 20 GB backups

DynamoDB Free Tier (per month):
├── 25 GB storage
├── 25 provisioned write capacity
├── 25 provisioned read capacity
└── 1 GB data transfer

After free tier:
RDS: Pay for instance + storage
DynamoDB: Pay per request
```

---

## Interview Questions

**Q: What's RDS?**
A: RDS is a managed relational database service. AWS handles backups, patches, and maintenance.

**Q: What's DynamoDB?**
A: DynamoDB is a fully managed NoSQL database with flexible schema and automatic scaling.

**Q: When use RDS vs DynamoDB?**
A: RDS for structured data with SQL. DynamoDB for flexible/real-time data with high throughput.

**Q: What's a partition key?**
A: The primary identifier for items in DynamoDB. Used for distributing data across partitions.

**Q: Can you query RDS without SQL?**
A: Not easily. RDS is SQL-based. For NoSQL, use DynamoDB.

---

## Cleanup

```bash
# Delete RDS instance
aws rds delete-db-instance \
  --db-instance-identifier mylearningdb \
  --skip-final-snapshot

# Delete DynamoDB table
aws dynamodb delete-table --table-name Users
```

---

## Next Steps
✅ Complete database projects
⬜ Move to **Project 5: Serverless Full-Stack App**

---

Last updated: June 29, 2026
