# Project 1: EC2 Basics - Launch Your First Web Server 🖥️

## Overview
Learn to launch, configure, and manage an EC2 instance. Deploy a simple web server and access it from the internet.

**Time**: 1.5-2 hours
**Cost**: FREE (within Free Tier)
**Difficulty**: 🟢 Beginner

---

## What You'll Learn
- [ ] Launch an EC2 instance
- [ ] Configure Security Groups (firewall)
- [ ] Connect via SSH/RDP
- [ ] Install a web server
- [ ] Host a simple web page
- [ ] Stop and terminate instances

---

## Prerequisites
- AWS Free Tier Account
- Terminal/SSH client (Mac/Linux) or PuTTY (Windows)
- 30 minutes free time

---

## Step-by-Step Guide

### Step 1: Launch an EC2 Instance

1. **Go to AWS Console**: https://console.aws.amazon.com/
2. **Search for EC2** → Click on "EC2"
3. **Click "Instances"** on left sidebar
4. **Click "Launch instances"** button

```
Instance Details:
├── Name: my-web-server
├── AMI (Image): Ubuntu Server 22.04 LTS (Free Tier eligible)
├── Instance Type: t2.micro (FREE)
├── Key Pair: Create new or use existing
│   └── Name: my-ec2-key
│   └── Type: RSA
│   └── Format: .pem (Mac/Linux) or .ppk (Windows)
├── VPC: Default VPC
├── Security Group: Create new
│   └── Name: my-web-sg
│   └── Rules (see below)
└── Storage: 8 GB gp3 (default, FREE)
```

### Step 2: Configure Security Group

Security Group = Firewall for your instance

Add these inbound rules:

```
Rule 1: SSH
├── Type: SSH
├── Protocol: TCP
├── Port Range: 22
├── Source: 0.0.0.0/0 (anywhere)
└── Description: Allow SSH access

Rule 2: HTTP
├── Type: HTTP
├── Protocol: TCP
├── Port Range: 80
├── Source: 0.0.0.0/0 (anywhere)
└── Description: Allow web traffic

Rule 3: HTTPS
├── Type: HTTPS
├── Protocol: TCP
├── Port Range: 443
├── Source: 0.0.0.0/0 (anywhere)
└── Description: Allow secure web traffic
```

### Step 3: Launch & Get Key Pair

1. **Review settings** → Click "Launch instance"
2. **Download key pair** (my-ec2-key.pem)
3. **Save it safely** - you'll need this to connect!

```bash
# On Mac/Linux: Change permissions
chmod 400 my-ec2-key.pem

# On Windows: Use PuTTYgen to convert .pem to .ppk
```

### Step 4: Connect to Instance

1. **Go to Instances** → Select your instance
2. **Get Public IP Address** (e.g., 54.123.45.67)
3. **Click "Connect"** → Choose "SSH client"

**Mac/Linux**:
```bash
ssh -i my-ec2-key.pem ubuntu@54.123.45.67
# Type 'yes' when asked to continue connecting
```

**Windows (Using PuTTY)**:
- Open PuTTY
- Host: 54.123.45.67
- Connection → SSH → Auth → Select my-ec2-key.ppk
- Click "Open"

### Step 5: Install Web Server

Once connected to your instance:

```bash
# Update system packages
sudo apt-get update
sudo apt-get upgrade -y

# Install Apache web server
sudo apt-get install apache2 -y

# Start Apache
sudo systemctl start apache2

# Enable Apache to start on boot
sudo systemctl enable apache2
```

### Step 6: Create a Simple Web Page

```bash
# Create index.html
sudo nano /var/www/html/index.html
```

Paste this content:
```html
<!DOCTYPE html>
<html>
<head>
    <title>My First AWS Web Server</title>
    <style>
        body { font-family: Arial; text-align: center; padding: 50px; }
        h1 { color: #FF9900; }
    </style>
</head>
<body>
    <h1>🎉 My First EC2 Web Server!</h1>
    <p>This is running on AWS EC2 instance</p>
    <p>Instance ID: <code>i-xxxxx</code></p>
    <p>Availability Zone: us-east-1a</p>
</body>
</html>
```

Save: `Ctrl+O` → Enter → `Ctrl+X`

### Step 7: Access Your Web Server

1. **Get Public IP** from AWS Console
2. **Open browser**: http://54.123.45.67
3. **See your web page!** ✅

---

## 🛠️ Useful Commands

```bash
# Check if Apache is running
sudo systemctl status apache2

# Restart Apache after changes
sudo systemctl restart apache2

# View Apache logs
sudo tail -f /var/log/apache2/access.log

# Check disk usage
df -h

# Check memory usage
free -h

# Disconnect from instance
exit
```

---

## AWS CLI Commands

```bash
# List all instances
aws ec2 describe-instances --region us-east-1

# Stop instance (keeps data)
aws ec2 stop-instances --instance-ids i-xxxxx --region us-east-1

# Start instance
aws ec2 start-instances --instance-ids i-xxxxx --region us-east-1

# Terminate instance (deletes everything)
aws ec2 terminate-instances --instance-ids i-xxxxx --region us-east-1
```

---

## 💰 Cost Management

```
EC2 t2.micro (Free Tier):
├── Usage: 750 hours/month (entire month)
├── Cost: FREE (if within free tier)
└── Per hour after: $0.0116/hour

⚠️ IMPORTANT:
- Stop instances when not using (no cost while stopped)
- Don't leave running 24/7 unless needed
- Delete after learning to avoid charges
```

---

## 📝 What's Happening Behind the Scenes?

```
Your Computer
    ↓ (SSH connection on port 22)
    ↓
Security Group (firewall)
    ├─ Port 22: ✅ SSH allowed
    ├─ Port 80: ✅ HTTP allowed
    └─ Port 443: ✅ HTTPS allowed
    ↓
EC2 Instance (Ubuntu Server)
    ├─ Operating System
    ├─ Apache Web Server (listens on port 80)
    └─ Your web page (index.html)
```

---

## Common Issues & Solutions

### ❌ "Connection refused"
**Cause**: Security group doesn't allow SSH
**Fix**: Edit Security Group → Add SSH rule (port 22)

### ❌ "Permission denied"
**Cause**: Key pair permissions wrong
**Fix**: `chmod 400 my-ec2-key.pem`

### ❌ "Can't access web page"
**Cause**: Apache not running or Security Group blocks HTTP
**Fix**: 
```bash
sudo systemctl start apache2
# Or add HTTP rule to Security Group
```

### ❌ "Public IP shows as pending"
**Cause**: Instance still starting up
**Fix**: Wait 1-2 minutes and refresh

---

## 🎯 Interview Questions

**Q: What is EC2?**
A: EC2 (Elastic Compute Cloud) is a web service that provides resizable computing capacity in the cloud. It's a virtual machine.

**Q: What's a Security Group?**
A: It's a virtual firewall that controls inbound and outbound traffic to your instance.

**Q: What's an AMI?**
A: Amazon Machine Image - a template that contains the OS and software needed to launch an instance.

**Q: Difference between stopping and terminating?**
A: Stopping keeps your data and EBS volumes (can restart). Terminating deletes everything.

**Q: How do you connect to EC2?**
A: Via SSH (Linux/Mac) or RDP (Windows) using a key pair.

---

## ✅ Cleanup (Important!)

After learning, clean up to avoid charges:

```bash
# 1. Stop instance
aws ec2 stop-instances --instance-ids i-xxxxx

# 2. After confirming it's stopped, terminate
aws ec2 terminate-instances --instance-ids i-xxxxx

# 3. Delete security group (after instance is terminated)
aws ec2 delete-security-group --group-id sg-xxxxx

# 4. Delete key pair (optional)
aws ec2 delete-key-pair --key-name my-ec2-key
```

Or use AWS Console:
1. Select instance → Instance State → Terminate
2. Confirm termination
3. Delete security group
4. Delete key pair

---

## 📚 Learning Resources

- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)

---

## Next Steps

✅ Complete this project
⬜ Move to **Project 2: Lambda Functions**
⬜ Then: **Project 3: S3 Storage**

---

Last updated: June 29, 2026
