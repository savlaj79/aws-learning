# Project 2: Lambda Functions - Serverless Computing 🚀

## Overview
Create and deploy AWS Lambda functions without managing servers. Build event-driven applications.

**Time**: 1-1.5 hours
**Cost**: FREE (within Free Tier)
**Difficulty**: 🟢 Beginner

---

## What You'll Learn
- [ ] Create Lambda functions
- [ ] Write Python/Node.js code
- [ ] Trigger functions with events
- [ ] Use CloudWatch logs
- [ ] Understand Lambda pricing
- [ ] Deploy and test functions

---

## What is Lambda?

**Lambda** = Run code without managing servers

```
Traditional Server:
┌─────────────────────────────┐
│ Server (EC2)                │
│ ├─ Operating System         │
│ ├─ Runtime (Python, Node)   │
│ ├─ Your Code                │
│ └─ Security patches, etc    │
└─────────────────────────────┘
You manage everything

Lambda:
┌─────────────────────────────┐
│ Lambda                      │
│ ├─ AWS manages OS, runtime  │
│ └─ You upload code only     │
└─────────────────────────────┘
AWS manages infrastructure
```

---

## Project 1: Hello World Lambda

### Step 1: Create Lambda Function

1. **Go to AWS Console** → Search "Lambda"
2. **Click "Create function"**
3. **Configure**:
   ```
   Function name: hello-world
   Runtime: Python 3.11
   Role: Create new role (basic execution)
   ```
4. **Click "Create function"**

### Step 2: Write Code

Replace the code with:

```python
def lambda_handler(event, context):
    """
    Lambda function handler
    event: Input data
    context: Runtime info
    """
    
    name = event.get('name', 'World')
    
    return {
        'statusCode': 200,
        'body': f'Hello, {name}! This is Lambda!'
    }
```

### Step 3: Test Function

1. **Click "Test"**
2. **Create test event**:
   ```json
   {
     "name": "AWS Learner"
   }
   ```
3. **Click "Test"**
4. **See output**:
   ```json
   {
     "statusCode": 200,
     "body": "Hello, AWS Learner! This is Lambda!"
   }
   ```

---

## Project 2: Lambda with S3 Trigger

### Scenario
When you upload a file to S3, Lambda automatically processes it.

### Step 1: Create Lambda Function

```python
import json
import boto3

s3_client = boto3.client('s3')

def lambda_handler(event, context):
    """
    Triggered when file uploaded to S3
    """
    
    # Get bucket and file info from event
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    
    print(f"Processing file: {key} from bucket: {bucket}")
    
    return {
        'statusCode': 200,
        'body': json.dumps(f'Processed {key}')
    }
```

### Step 2: Add S3 Trigger

1. **In Lambda function**
2. **Click "Add trigger"**
3. **Select S3**
4. **Configure**:
   ```
   Bucket: your-bucket-name
   Event type: s3:ObjectCreated:*
   ```
5. **Add trigger**

### Step 3: Test

1. **Upload file to S3**
2. **Lambda automatically triggers**
3. **Check CloudWatch Logs** for output

---

## Project 3: Lambda with DynamoDB

### Scenario
Lambda reads/writes data to DynamoDB database.

### Step 1: Create DynamoDB Table

1. **Go to DynamoDB** → "Create table"
2. **Configure**:
   ```
   Table name: users
   Partition key: user_id (String)
   Billing: On-demand (Free tier friendly)
   ```
3. **Create table**

### Step 2: Create Lambda Function

```python
import json
import boto3
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('users')

def lambda_handler(event, context):
    """
    Save user data to DynamoDB
    """
    
    try:
        user_id = event['user_id']
        name = event['name']
        email = event['email']
        
        # Put item in DynamoDB
        table.put_item(
            Item={
                'user_id': user_id,
                'name': name,
                'email': email,
                'created_at': str(datetime.now())
            }
        )
        
        return {
            'statusCode': 200,
            'body': json.dumps(f'User {name} created successfully')
        }
    
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps(f'Error: {str(e)}')
        }
```

### Step 3: Add Permission

1. **In Lambda** → **Permissions** tab
2. **Click "Add permissions"**
3. **Grant DynamoDB access**

### Step 4: Test

```json
{
  "user_id": "user123",
  "name": "John Doe",
  "email": "john@example.com"
}
```

---

## Lambda Pricing & Free Tier

```
Free Tier (per month):
├── 1,000,000 requests (always free)
├── 400,000 GB-seconds of compute time
│   └── GB-second = 1 GB of memory for 1 second
└── Free tier doesn't expire

After free tier:
├── Requests: $0.20 per million
└── Compute: $0.0000166667 per GB-second

Example:
100,000 requests × 512 MB × 1 second
= $0.001 (very cheap!)
```

---

## AWS CLI Commands

```bash
# List Lambda functions
aws lambda list-functions

# Create function
aws lambda create-function \
  --function-name hello-world \
  --runtime python3.11 \
  --role arn:aws:iam::ACCOUNT:role/service-role/lambda-role \
  --handler index.lambda_handler \
  --zip-file fileb://function.zip

# Invoke function
aws lambda invoke \
  --function-name hello-world \
  --payload '{"name": "AWS"}' \
  response.json

# Get logs
aws logs tail /aws/lambda/hello-world --follow

# Delete function
aws lambda delete-function --function-name hello-world
```

---

## 🛠️ Best Practices

✅ **Do**:
- Keep functions small and focused
- Use environment variables for config
- Monitor with CloudWatch
- Set timeout appropriately (max 15 minutes)
- Handle errors gracefully

❌ **Don't**:
- Run large computations in Lambda
- Store data in /tmp (not persistent)
- Keep connections open between invocations
- Log sensitive data

---

## Interview Questions

**Q: What is Lambda?**
A: AWS Lambda is a serverless compute service that runs code in response to events without provisioning servers.

**Q: How is Lambda different from EC2?**
A: EC2 requires managing servers. Lambda is fully managed - you just upload code. Lambda is cheaper for short tasks.

**Q: What's a Lambda handler?**
A: The function that AWS calls. It receives two parameters: event (input) and context (runtime info).

**Q: What triggers Lambda?**
A: Events like S3 uploads, API calls, DynamoDB streams, SNS messages, scheduled events, etc.

**Q: What's the maximum timeout?**
A: 15 minutes (900 seconds). After that, Lambda terminates the function.

---

## Cleanup

```bash
# Delete Lambda function
aws lambda delete-function --function-name hello-world

# Delete DynamoDB table
aws dynamodb delete-table --table-name users
```

---

## Next Steps
✅ Complete Lambda projects
⬜ Move to **Project 3: S3 Storage**
⬜ Then: **Project 4: Database (RDS/DynamoDB)**

---

Last updated: June 29, 2026
