# Project 3: S3 Storage & CloudFront 💾

## Overview
Create S3 buckets, upload files, manage versioning, and set up CloudFront CDN for fast content delivery.

**Time**: 1.5-2 hours
**Cost**: FREE (within Free Tier)
**Difficulty**: 🟢 Beginner

---

## What You'll Learn
- [ ] Create S3 buckets
- [ ] Upload and manage objects
- [ ] Configure access control
- [ ] Enable versioning
- [ ] Set up lifecycle policies
- [ ] Configure CloudFront CDN
- [ ] Host static website

---

## What is S3?

**S3** = Simple Storage Service (object storage)

```
S3 Bucket Structure:

my-bucket/
├── images/
│   ├── photo1.jpg
│   ├── photo2.jpg
│   └── logo.png
├── documents/
│   ├── resume.pdf
│   └── cover-letter.docx
└── index.html

Each file = Object
Container = Bucket
Bucket name must be globally unique
```

---

## Project 1: Create & Manage S3 Bucket

### Step 1: Create Bucket

1. **Go to AWS Console** → Search "S3"
2. **Click "Create bucket"**
3. **Configure**:
   ```
   Bucket name: my-learning-bucket-RANDOMNUMBER
   Region: us-east-1
   Block public access: Uncheck (to make public)
   Versioning: Enable
   Encryption: Default (SSE-S3)
   ```
4. **Create bucket**

### Step 2: Upload Files

1. **Click bucket**
2. **Click "Upload"**
3. **Drag & drop files** (images, documents, etc)
4. **Click "Upload"**

### Step 3: Make Objects Public

1. **Select file**
2. **Click "Make public using ACL"** or
3. **Use bucket policy**:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### Step 4: Access via URL

```
https://my-bucket.s3.amazonaws.com/photo.jpg
```

---

## Project 2: S3 Versioning & Lifecycle

### What is Versioning?
Keep multiple versions of objects. Recover deleted or changed files.

### Enable Versioning

1. **Go to bucket**
2. **Properties**
3. **Versioning** → **Enable**
4. **Save changes**

### Upload Same File Twice

```
Versions of document.pdf:
├── Version 1 (original)
├── Version 2 (updated)
└── Version 3 (latest)

You can restore any version
```

### Lifecycle Policy

Automatically delete old files to save cost:

1. **Management** tab
2. **Lifecycle rules**
3. **Create rule**:
   ```
   Rule name: delete-old-files
   Apply to: All objects
   
   Actions:
   ├── Delete non-current versions after: 30 days
   └── Delete objects: After 90 days
   ```

---

## Project 3: Static Website Hosting

### Create Static Website

1. **Create HTML file** (index.html):

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Portfolio</title>
    <style>
        body {
            font-family: Arial;
            max-width: 800px;
            margin: 50px auto;
            padding: 20px;
            background: #f5f5f5;
        }
        h1 { color: #FF9900; }
    </style>
</head>
<body>
    <h1>Welcome to My Portfolio</h1>
    <p>Hosted on AWS S3 ✨</p>
    <h2>Skills</h2>
    <ul>
        <li>AWS Cloud</li>
        <li>Python</li>
        <li>Web Development</li>
    </ul>
</body>
</html>
```

2. **Upload to S3**
3. **Enable static website hosting**:
   - **Properties** → **Static website hosting**
   - Index document: index.html
   - Error document: error.html (optional)
4. **Get website URL**:
   ```
   http://my-bucket.s3-website-us-east-1.amazonaws.com
   ```

---

## Project 4: CloudFront CDN

### What is CloudFront?
Globally distributed content delivery network (CDN). Caches and serves content from servers near users.

```
Without CloudFront:
User in Singapore
    ↓ (9000+ km)
S3 in us-east-1
⚠️ Slow!

With CloudFront:
User in Singapore
    ↓ (100 km to nearest edge location)
CloudFront Edge Location (Singapore)
    ↓ (first time, fetches from S3)
S3 in us-east-1
✅ Fast! (cached)
```

### Create CloudFront Distribution

1. **Go to CloudFront**
2. **Create distribution**
3. **Configure**:
   ```
   Origin: Select your S3 bucket
   Origin access: OAI (Origin Access Identity)
   Default root object: index.html
   Compress objects automatically: Yes
   ```
4. **Create distribution**

### Access via CloudFront

```
CloudFront URL:
d123456.cloudfront.net

Access: https://d123456.cloudfront.net/photo.jpg
```

---

## AWS CLI Commands

```bash
# Create bucket
aws s3 mb s3://my-bucket

# Upload file
aws s3 cp photo.jpg s3://my-bucket/

# Upload entire directory
aws s3 cp ./my-folder s3://my-bucket/ --recursive

# List bucket contents
aws s3 ls s3://my-bucket/ --recursive

# Download file
aws s3 cp s3://my-bucket/photo.jpg ./photo.jpg

# Delete object
aws s3 rm s3://my-bucket/photo.jpg

# Delete bucket (must be empty)
aws s3 rb s3://my-bucket
```

---

## S3 Pricing

```
Free Tier (per month):
├── 5 GB of storage
├── 20,000 GET requests
├── 2,000 PUT requests
└── 100 GB data transfer (out)

After free tier:
├── Storage: $0.023 per GB
├── Requests: $0.0004 per 1,000 requests
└── Data transfer: $0.09 per GB
```

---

## S3 Storage Classes

| Class | Use Case | Cost |
|-------|----------|------|
| **Standard** | Frequent access | Normal |
| **Intelligent-Tiering** | Unknown access | Auto-optimized |
| **Standard-IA** | Infrequent access (30+ days) | Cheaper |
| **Glacier** | Archive (90+ days) | Very cheap |
| **Deep Archive** | Long-term archive | Cheapest |

---

## Interview Questions

**Q: What is S3?**
A: S3 (Simple Storage Service) is object storage. It stores files as objects in buckets.

**Q: What's the max object size?**
A: 5 TB. For larger uploads, use multipart upload.

**Q: How do you make S3 objects public?**
A: Use bucket policy or ACL. Unblock public access first.

**Q: What's the difference between S3 and EBS?**
A: S3 is object storage (files). EBS is block storage (persistent disk for EC2).

**Q: How does CloudFront work?**
A: Caches content at 400+ edge locations worldwide. Delivers content from servers near users.

---

## Cleanup

```bash
# Delete all objects
aws s3 rm s3://my-bucket --recursive

# Delete bucket
aws s3 rb s3://my-bucket

# Delete CloudFront distribution
aws cloudfront delete-distribution --id D123456
```

---

## Next Steps
✅ Complete S3 projects
⬜ Move to **Project 4: Databases**
⬜ Then: **Project 5: Serverless App**

---

Last updated: June 29, 2026
