# Hands-On Practice Labs

[← Back to Study Plan](06-study-plan.md) | [Return to Main Guide →](README.md)

---

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Important Notes](#important-notes)
- [Lab 1: Set Up Billing Alerts and Budget](#lab-1-set-up-billing-alerts-and-budget)
- [Lab 2: IAM Users, Groups, Roles, and MFA](#lab-2-iam-users-groups-roles-and-mfa)
- [Lab 3: Launch and Configure EC2 Instance](#lab-3-launch-and-configure-ec2-instance)
- [Lab 4: Amazon S3 Storage and Website Hosting](#lab-4-amazon-s3-storage-and-website-hosting)
- [Lab 5: VPC, Subnets, and Network Configuration](#lab-5-vpc-subnets-and-network-configuration)
- [Lab 6: Amazon RDS Database](#lab-6-amazon-rds-database)
- [Lab 7: CloudWatch Monitoring and Alarms](#lab-7-cloudwatch-monitoring-and-alarms)
- [Lab 8: AWS Cost Management Tools](#lab-8-aws-cost-management-tools)
- [Lab 9: Lambda Serverless Function](#lab-9-lambda-serverless-function)
- [Lab 10: CloudFormation Infrastructure as Code](#lab-10-cloudformation-infrastructure-as-code)
- [Additional Practice Recommendations](#additional-practice-recommendations)

---

## Introduction

> **Important:** Hands-on experience is crucial for exam success. These labs will help you understand AWS services beyond theory and are designed to stay within Free Tier limits when done carefully.

Practical experience with AWS services provides:
- Deeper understanding of service capabilities
- Familiarity with AWS Management Console
- Confidence during the exam
- Real-world skills applicable to jobs
- Better retention of concepts

**Total Time Investment:** Approximately 6-7 hours for all labs

---

## Prerequisites

### Create Your AWS Account

Follow these steps to create your AWS account:

1. Visit [https://aws.amazon.com](https://aws.amazon.com)
2. Click **"Create an AWS Account"**
3. Provide:
   - Email address
   - Password
   - AWS account name
4. Enter contact information
5. Provide payment method
   - Credit or debit card required
   - You won't be charged if you stay within Free Tier
6. Verify identity via phone call or SMS
7. Select Support Plan: **Basic (Free)**
8. Wait for account activation (can take up to 24 hours)
9. Check email for confirmation

### AWS Free Tier

**Duration:** 12 months from account creation date

**Key Free Tier Services:**
- **EC2:** 750 hours/month of t2.micro or t3.micro instances
- **S3:** 5 GB of standard storage
- **RDS:** 750 hours/month of db.t2.micro, db.t3.micro, or db.t4g.micro
- **Lambda:** 1 million free requests per month
- **CloudWatch:** 10 custom metrics and alarms

**Always Free Services:**
- DynamoDB: 25 GB storage
- Lambda: 1 million requests/month
- CloudFormation: No charge (pay for resources created)

> **Exam Tip:** Set up billing alerts immediately to avoid unexpected charges. AWS Free Tier is generous, but mistakes can incur costs.

---

## Important Notes

### Safety and Cost Management

1. **Always clean up resources** after each lab to avoid charges
2. **Set billing alerts** before starting any hands-on work
3. **Use t2.micro or t3.micro** instance types (Free Tier eligible)
4. **Choose Free Tier regions** (us-east-1, us-west-2 recommended)
5. **Monitor Free Tier usage** in billing dashboard regularly
6. **Don't leave resources running** overnight or when not in use

### Best Practices

- Complete labs in order (dependencies exist)
- Take notes and screenshots for future reference
- Read error messages carefully - they often contain solutions
- Use tags to identify lab resources for easy cleanup
- Stop services rather than terminate if you plan to return

### Troubleshooting Resources

- AWS Documentation: [https://docs.aws.amazon.com](https://docs.aws.amazon.com)
- AWS re:Post (community forum): [https://repost.aws](https://repost.aws)
- Service Health Dashboard: [https://status.aws.amazon.com](https://status.aws.amazon.com)
- AWS Support (if you have paid support plan)

---

## Lab 1: Set Up Billing Alerts and Budget

**Duration:** 15 minutes
**Cost:** Free
**Difficulty:** Beginner

### Objective

Protect yourself from unexpected charges by setting up billing alerts and budgets.

### Prerequisites

- Active AWS account
- Access to root account or IAM user with billing permissions

### Step-by-Step Instructions

#### Part 1: Enable Billing Alerts

1. Sign in to **AWS Management Console**
2. Click your **account name** (top right) → **"Account"**
3. Scroll to **"IAM User and Role Access to Billing Information"**
4. Click **"Edit"**
5. Enable **"Activate IAM Access"**
6. Click **"Update"**

#### Part 2: Create CloudWatch Billing Alarm

1. Navigate to **CloudWatch** service
2. Select region: **N. Virginia (us-east-1)** (billing metrics only in us-east-1)
3. Go to **"Alarms"** → **"Billing"** → **"Create alarm"**
4. Click **"Select metric"**
5. Select **"Billing"** → **"Total Estimated Charge"**
6. Select **USD** checkbox
7. Click **"Select metric"**
8. Configure alarm:
   - **Threshold type:** Static
   - **Whenever:** Greater than
   - **Amount:** $5 (or your preferred threshold)
9. Click **"Next"**

#### Part 3: Configure SNS Notification

1. Under **"Notification"**:
   - **Select an SNS topic:** Create new topic
   - **Topic name:** "Billing-Alerts"
   - **Email endpoints:** Enter your email address
2. Click **"Create topic"**
3. Click **"Next"**
4. **Alarm name:** "Monthly-Billing-Alert"
5. **Alarm description:** "Alert when monthly charges exceed $5"
6. Click **"Next"**
7. Review and click **"Create alarm"**
8. **Check your email** and click confirmation link

#### Part 4: Create AWS Budget

1. Navigate to **"Billing and Cost Management"**
2. Click **"Budgets"** in left menu
3. Click **"Create budget"**
4. Select **"Cost budget"** → **"Next"**
5. Configure budget:
   - **Budget name:** "Monthly-Cost-Budget"
   - **Period:** Monthly
   - **Budget amount:** $10
   - **Budget scope:** All AWS services
6. Click **"Next"**
7. Configure alerts:
   - **Alert 1:** 80% of budgeted amount
   - **Email recipients:** Your email
   - **Alert 2:** 100% of budgeted amount
8. Click **"Next"**
9. Review and click **"Create budget"**

### Expected Outcomes

- CloudWatch billing alarm created and active
- Email confirmation received for SNS subscription
- Budget created with two alert thresholds
- Email notifications configured

### Verification

1. Go to **CloudWatch** → **Alarms** → Verify alarm shows "OK" state
2. Go to **Budgets** → Verify budget shows current spend vs. budget
3. Check email for SNS confirmation

### Troubleshooting

**Problem:** Billing metric not showing
**Solution:** Ensure you're in us-east-1 region; billing metrics only available there

**Problem:** Email confirmation not received
**Solution:** Check spam folder; resend confirmation from SNS console

**Problem:** Can't access billing information
**Solution:** Enable IAM access to billing in Account settings

### Cleanup

> **Note:** Keep these alerts active for ongoing protection. No cleanup needed.

---

## Lab 2: IAM Users, Groups, Roles, and MFA

**Duration:** 30 minutes
**Cost:** Free
**Difficulty:** Beginner

### Objective

Understand IAM security best practices by creating users, groups, roles, and enabling MFA.

### Prerequisites

- AWS account with root access
- Smartphone with authenticator app (Google Authenticator, Authy, etc.)

### Step-by-Step Instructions

#### Part 1: Secure Root Account with MFA

1. Navigate to **IAM** service in AWS Console
2. Click **"Dashboard"** → Review security recommendations
3. Click **"Add MFA"** for root account
4. **MFA device name:** "root-mfa-device"
5. Select **"Virtual MFA device"** → **"Next"**
6. Install authenticator app on your phone:
   - Google Authenticator (iOS/Android)
   - Authy (iOS/Android)
   - Microsoft Authenticator
7. Click **"Show QR code"**
8. Scan QR code with authenticator app
9. Enter **two consecutive MFA codes** from app
10. Click **"Add MFA"**
11. **Verify:** MFA badge appears on dashboard

> **Important:** Store root account credentials securely and use IAM users for daily tasks.

#### Part 2: Create IAM Admin User

1. In IAM, click **"Users"** → **"Create user"**
2. **User name:** "admin-user"
3. Select **"Provide user access to the AWS Management Console"**
4. Choose **"I want to create an IAM user"**
5. Password options:
   - Custom password OR Autogenerated
   - Uncheck "Users must create a new password at next sign-in"
6. Click **"Next"**
7. **Permissions options:** "Attach policies directly"
8. Search and select **"AdministratorAccess"**
9. Click **"Next"**
10. Review and click **"Create user"**
11. **Download .csv file** (contains credentials)
12. **Copy console sign-in URL** (save for later)

#### Part 3: Create IAM Groups

**Create Developers Group:**

1. Click **"User groups"** → **"Create group"**
2. **Group name:** "Developers"
3. **Attach permissions policies:**
   - Search and select **"AmazonEC2ReadOnlyAccess"**
   - Search and select **"AmazonS3FullAccess"**
4. Click **"Create group"**

**Create Administrators Group:**

1. Click **"Create group"** again
2. **Group name:** "Administrators"
3. **Attach permissions policy:**
   - Search and select **"AdministratorAccess"**
4. Click **"Create group"**

#### Part 4: Create Additional IAM Users

**Create Developer User 1:**

1. Click **"Users"** → **"Create user"**
2. **User name:** "developer-1"
3. Enable **console access**
4. Set password (custom or autogenerated)
5. Click **"Next"**
6. **Add user to groups:** Select **"Developers"** group
7. Click **"Next"** → **"Create user"**

**Create Developer User 2:**

1. Repeat above steps
2. **User name:** "developer-2"
3. Add to **"Developers"** group
4. Create user

#### Part 5: Create IAM Role for EC2

1. Click **"Roles"** → **"Create role"**
2. **Trusted entity type:** "AWS service"
3. **Use case:** Select **"EC2"**
4. Click **"Next"**
5. **Attach permissions:**
   - Search and select **"AmazonS3ReadOnlyAccess"**
6. Click **"Next"**
7. **Role name:** "EC2-S3-ReadOnly-Role"
8. **Description:** "Allows EC2 instances to read from S3"
9. Click **"Create role"**

#### Part 6: Test IAM Policies

1. **Sign out** from root account
2. **Sign in** as "developer-1" using:
   - Console sign-in URL (saved earlier)
   - Username: developer-1
   - Password: (as set)
3. Try to access **S3** service (should work - full access)
4. Try to create S3 bucket (should work)
5. Try to access **IAM** service (should be denied - no permission)
6. Try to view **EC2** instances (should work - read-only)
7. Try to launch EC2 instance (should be denied - read-only)
8. **Sign out**

### Expected Outcomes

- Root account secured with MFA
- IAM admin user created with full permissions
- Two IAM groups created (Developers, Administrators)
- Two developer users created and added to Developers group
- IAM role created for EC2 to access S3
- Successfully tested permissions and restrictions

### Verification

1. **IAM Dashboard** shows:
   - MFA enabled for root
   - Multiple users created
   - Groups with attached policies
   - Role created
2. **Sign-in test** as developer-1 confirms permission boundaries

### Troubleshooting

**Problem:** Can't sign in as IAM user
**Solution:** Use account-specific sign-in URL, not root login page

**Problem:** MFA setup fails
**Solution:** Ensure phone time is synchronized; try re-scanning QR code

**Problem:** Permission denied errors
**Solution:** Verify user is in correct group; check group policies attached

### Cleanup

> **Note:** Keep admin-user for future labs. Can delete developer users and groups if desired.

**To delete users:**
1. Select user → **"Delete"** → Confirm

**To delete groups:**
1. Remove all users from group first
2. Select group → **"Delete"** → Confirm

**To delete role:**
1. Select role → **"Delete"** → Confirm

---

## Lab 3: Launch and Configure EC2 Instance

**Duration:** 45 minutes
**Cost:** Free (t2.micro/t3.micro in Free Tier)
**Difficulty:** Intermediate

### Objective

Launch a web server on EC2, connect via SSH, create an AMI, and take snapshots.

### Prerequisites

- AWS account
- IAM user with EC2 permissions
- Basic command line knowledge

### Step-by-Step Instructions

#### Part 1: Launch EC2 Instance

1. Navigate to **EC2** service
2. Select region: **us-east-1** (or your preferred region)
3. Click **"Launch instance"**
4. **Name:** "MyWebServer"
5. **Application and OS Images (AMI):**
   - Select **"Amazon Linux 2023 AMI"**
   - Verify "Free tier eligible" label
6. **Instance type:**
   - Select **"t2.micro"** or **"t3.micro"** (Free tier eligible)
7. **Key pair:**
   - Click **"Create new key pair"**
   - **Key pair name:** "my-key-pair"
   - **Key pair type:** RSA
   - **Private key file format:**
     - **.pem** (for Mac/Linux/Windows OpenSSH)
     - **.ppk** (for Windows PuTTY)
   - Click **"Create key pair"**
   - **Save file securely** (you can't download again)

#### Part 2: Configure Network Settings

1. **Network settings:**
   - Click **"Edit"**
   - **VPC:** Default VPC
   - **Subnet:** No preference
   - **Auto-assign public IP:** Enable
2. **Firewall (Security groups):**
   - Select **"Create security group"**
   - **Security group name:** "web-server-sg"
   - **Description:** "Allow SSH and HTTP"
   - **Inbound security group rules:**
     - **Rule 1:**
       - Type: SSH
       - Protocol: TCP
       - Port: 22
       - Source: My IP
     - Click **"Add security group rule"**
     - **Rule 2:**
       - Type: HTTP
       - Protocol: TCP
       - Port: 80
       - Source: 0.0.0.0/0 (anywhere)

#### Part 3: Configure Storage and User Data

1. **Configure storage:**
   - **Size:** 8 GiB (default)
   - **Volume type:** gp3 (default)
   - Keep other defaults
2. **Expand "Advanced details"**
3. Scroll to **"User data"**
4. Paste the following script:

```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from AWS EC2</h1>" > /var/www/html/index.html
```

5. Click **"Launch instance"**
6. Wait for **"Successfully initiated launch"** message
7. Click **"View all instances"**

#### Part 4: Connect to Instance

**Wait for instance to be ready:**
- **Instance state:** Running
- **Status check:** 2/2 checks passed (may take 2-3 minutes)

**Get connection information:**
1. Select your instance
2. Copy **"Public IPv4 address"**
3. Test web server: Open browser, navigate to `http://[public-ip]`
4. You should see: **"Hello from AWS EC2"**

**Connect via SSH (Mac/Linux/Windows OpenSSH):**

1. Open terminal
2. Navigate to directory with key pair:
   ```bash
   cd ~/Downloads
   ```
3. Set correct permissions:
   ```bash
   chmod 400 my-key-pair.pem
   ```
4. Connect to instance:
   ```bash
   ssh -i my-key-pair.pem ec2-user@[public-ip-address]
   ```
5. Type **"yes"** to accept fingerprint
6. You're now connected!

**Connect via SSH (Windows PuTTY):**

1. Open PuTTY
2. **Host Name:** ec2-user@[public-ip]
3. **Port:** 22
4. **Connection → SSH → Auth:**
   - Browse and select .ppk file
5. Click **"Open"**
6. Accept security alert
7. You're connected!

**Verify web server:**
```bash
sudo systemctl status httpd
```

#### Part 5: Create AMI (Amazon Machine Image)

1. In EC2 Console, select your instance
2. Click **"Actions"** → **"Image and templates"** → **"Create image"**
3. **Image name:** "MyWebServer-AMI"
4. **Image description:** "Web server with Apache installed"
5. Keep other defaults
6. Click **"Create image"**
7. Go to **"AMIs"** in left navigation menu
8. Wait for **Status:** "Available" (takes 2-5 minutes)

> **Note:** You can now launch new instances from this AMI with Apache pre-installed.

#### Part 6: Create EBS Snapshot

1. Go to **"Volumes"** in EC2 left menu
2. Select volume attached to your instance (check "Attachment information")
3. Click **"Actions"** → **"Create snapshot"**
4. **Description:** "WebServer-backup"
5. **Tags:**
   - Key: Name
   - Value: WebServer-Snapshot
6. Click **"Create snapshot"**
7. Go to **"Snapshots"** to view status
8. Wait for **Status:** "Completed"

### Expected Outcomes

- EC2 instance running Amazon Linux 2023
- Apache web server installed and accessible via HTTP
- Successfully connected via SSH
- AMI created from running instance
- EBS snapshot created for backup

### Verification

1. **Web server accessible:** Visit `http://[public-ip]` → See "Hello from AWS EC2"
2. **SSH connection works:** Able to connect and run commands
3. **AMI created:** Shows in AMIs list with "Available" status
4. **Snapshot created:** Shows in Snapshots list with "Completed" status

### Troubleshooting

**Problem:** Can't access web server (timeout)
**Solution:**
- Verify security group allows HTTP (port 80) from 0.0.0.0/0
- Ensure instance is in "running" state
- Check status checks passed
- Verify you're using HTTP, not HTTPS

**Problem:** SSH connection refused
**Solution:**
- Verify security group allows SSH (port 22) from your IP
- Check you're using correct key pair file
- Ensure using "ec2-user" as username
- Verify key file has correct permissions (400)

**Problem:** Permission denied (publickey)
**Solution:**
- Verify using correct .pem file
- Check file permissions: `chmod 400 my-key-pair.pem`
- Ensure using correct username (ec2-user for Amazon Linux)

**Problem:** AMI creation fails
**Solution:**
- Ensure instance is in "running" or "stopped" state
- Check you have sufficient EBS snapshot quota

### Cleanup (Important!)

> **Critical:** Always clean up to avoid charges after Free Tier expires.

**Delete resources in this order:**

1. **Terminate instance:**
   - Select instance
   - **"Instance state"** → **"Terminate instance"**
   - Confirm termination
   - Wait for state: "Terminated"

2. **Delete snapshot:**
   - Go to **"Snapshots"**
   - Select your snapshot
   - **"Actions"** → **"Delete snapshot"**
   - Confirm deletion

3. **Deregister AMI:**
   - Go to **"AMIs"**
   - Select your AMI
   - **"Actions"** → **"Deregister AMI"**
   - Confirm deregistration

4. **Delete AMI snapshot:**
   - Go to **"Snapshots"**
   - Find snapshot created by AMI (check description)
   - **"Actions"** → **"Delete snapshot"**
   - Confirm deletion

5. **Delete key pair (optional):**
   - Go to **"Key Pairs"**
   - Select key pair
   - **"Actions"** → **"Delete"**
   - Confirm deletion

---

## Lab 4: Amazon S3 Storage and Website Hosting

**Duration:** 40 minutes
**Cost:** Free (within 5 GB Free Tier)
**Difficulty:** Intermediate

### Objective

Master S3 storage features including bucket creation, static website hosting, versioning, lifecycle policies, and encryption.

### Prerequisites

- AWS account
- Basic HTML knowledge
- Text editor

### Step-by-Step Instructions

#### Part 1: Create S3 Bucket

1. Navigate to **S3** service
2. Click **"Create bucket"**
3. **Bucket name:** "my-website-[yourname]-[random-numbers]"
   - Must be globally unique
   - Example: "my-website-john-12345"
   - Only lowercase letters, numbers, hyphens
4. **AWS Region:** Select your preferred region (us-east-1 recommended)
5. **Object Ownership:** ACLs disabled (recommended)
6. **Block Public Access settings:**
   - **Uncheck** "Block all public access"
   - **Check** acknowledgment box (needed for website hosting)
7. **Bucket Versioning:** Enable
8. **Tags:**
   - Key: Project
   - Value: Learning
9. **Default encryption:** Server-side encryption with Amazon S3 managed keys (SSE-S3)
10. Click **"Create bucket"**

#### Part 2: Upload HTML Files

**Create index.html file:**

1. Open text editor
2. Create file named `index.html`
3. Paste the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My AWS Website</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 50px auto;
            padding: 20px;
        }
        h1 { color: #FF9900; }
    </style>
</head>
<body>
    <h1>Welcome to My S3 Website</h1>
    <p>This website is hosted on Amazon S3!</p>
    <p>S3 provides durable, scalable object storage.</p>
</body>
</html>
```

4. Save file

**Create error.html file:**

1. Create file named `error.html`
2. Paste the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Error</title>
</head>
<body>
    <h1>404 - Page Not Found</h1>
    <p>The requested page doesn't exist.</p>
</body>
</html>
```

3. Save file

**Upload files to S3:**

1. Click on your bucket name
2. Click **"Upload"**
3. Click **"Add files"**
4. Select `index.html` and `error.html`
5. Click **"Upload"**
6. Wait for upload to complete
7. Click **"Close"**

#### Part 3: Enable Static Website Hosting

1. In your bucket, go to **"Properties"** tab
2. Scroll to **"Static website hosting"**
3. Click **"Edit"**
4. **Static website hosting:** Enable
5. **Hosting type:** "Host a static website"
6. **Index document:** index.html
7. **Error document:** error.html
8. Click **"Save changes"**
9. Scroll back to "Static website hosting"
10. **Copy the "Bucket website endpoint" URL** (save for later)

#### Part 4: Configure Bucket Policy for Public Access

1. Go to **"Permissions"** tab
2. Scroll to **"Bucket policy"**
3. Click **"Edit"**
4. Paste the following policy (replace `YOUR-BUCKET-NAME` with your actual bucket name):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

5. Click **"Save changes"**
6. **Test website:** Open bucket website endpoint URL in browser
7. You should see your "Welcome to My S3 Website" page!

#### Part 5: Test Versioning

1. Edit `index.html` locally:
   - Change heading to "Welcome to My Updated S3 Website"
   - Add a line: `<p>This is version 2!</p>`
   - Save file
2. Upload new version to S3:
   - Go to bucket → **"Upload"**
   - Select modified `index.html`
   - **"Upload"**
3. View versions:
   - In bucket, select `index.html`
   - Click **"Versions"** tab
   - You'll see multiple versions listed
4. Click on older version to view/download
5. To restore older version:
   - Select older version
   - Click **"Actions"** → **"Download"**
   - Re-upload as new version

#### Part 6: Create Lifecycle Policy

1. Go to **"Management"** tab
2. Click **"Create lifecycle rule"**
3. **Lifecycle rule name:** "Archive-Old-Files"
4. **Rule scope:** Apply to all objects in the bucket
5. **Lifecycle rule actions** (check these):
   - Transition current versions of objects between storage classes
   - Expire current versions of objects
6. **Transition current versions:**
   - **Days after object creation:** 30
   - **Storage class:** Standard-IA
   - Click **"Add transition"**
   - **Days:** 90
   - **Storage class:** Glacier Flexible Retrieval
7. **Expire current versions of objects:**
   - **Days after object creation:** 365
8. **Acknowledge warning** about costs
9. Click **"Create rule"**

#### Part 7: Enable Server-Side Encryption

1. Go to **"Properties"** tab
2. Scroll to **"Default encryption"**
3. Click **"Edit"**
4. **Encryption type:** Server-side encryption with Amazon S3 managed keys (SSE-S3)
5. **Bucket Key:** Enabled (reduces encryption costs)
6. Click **"Save changes"**

### Expected Outcomes

- S3 bucket created with unique name
- Static website hosting enabled and accessible
- HTML files uploaded and viewable via HTTP
- Versioning enabled and tested
- Lifecycle policy created for automatic archival
- Encryption enabled for security

### Verification

1. **Website accessible:** Visit bucket endpoint URL → See your website
2. **Versioning works:** Multiple versions visible in Versions tab
3. **Lifecycle rule created:** Visible in Management tab
4. **Encryption enabled:** Shows in Properties tab

### Troubleshooting

**Problem:** 403 Forbidden error when accessing website
**Solution:**
- Verify bucket policy allows public read (s3:GetObject)
- Check "Block all public access" is OFF
- Ensure bucket policy has correct bucket name
- Verify ARN includes /* at end

**Problem:** 404 Not Found error
**Solution:**
- Ensure index.html is uploaded to bucket root
- Check filename is exactly "index.html" (case-sensitive)
- Verify static website hosting is enabled

**Problem:** Can't upload files
**Solution:**
- Check you have s3:PutObject permissions
- Verify bucket exists and you're in correct region
- Try uploading smaller files first

### Cleanup

> **Important:** Delete all objects before deleting bucket.

1. **Empty bucket:**
   - Select your bucket
   - Click **"Empty"**
   - Type **"permanently delete"**
   - Click **"Empty"**
   - Wait for completion

2. **Delete bucket:**
   - Select your bucket
   - Click **"Delete"**
   - Type bucket name to confirm
   - Click **"Delete bucket"**

---

## Lab 5: VPC, Subnets, and Network Configuration

**Duration:** 60 minutes
**Cost:** Free (NAT Gateway excluded to avoid charges)
**Difficulty:** Advanced

### Objective

Build a custom VPC with public and private subnets, configure routing, security groups, and NACLs.

### Prerequisites

- Understanding of networking basics (IP addresses, subnets, CIDR)
- Completed Lab 3 (EC2 knowledge required)

### Step-by-Step Instructions

#### Part 1: Create VPC

1. Navigate to **VPC** service
2. Click **"Create VPC"**
3. **Resources to create:** "VPC and more" (creates VPC with subnets automatically)
4. **Name tag auto-generation:** "MyVPC"
5. **IPv4 CIDR block:** 10.0.0.0/16
   - Provides 65,536 IP addresses
6. **IPv6 CIDR block:** No IPv6 CIDR block
7. **Tenancy:** Default
8. **Number of Availability Zones:** 2
9. **Number of public subnets:** 2
10. **Number of private subnets:** 2
11. **NAT gateways:** None (to stay in Free Tier)
    - **Important:** NAT Gateway costs $0.045/hour
12. **VPC endpoints:** None
13. **DNS options:**
    - Enable DNS hostnames: Yes
    - Enable DNS resolution: Yes
14. Click **"Create VPC"**
15. Wait for creation (takes 1-2 minutes)
16. Click **"View VPC"**

#### Part 2: Review VPC Components

**Review VPC:**
1. Go to **"Your VPCs"**
2. Verify VPC created with CIDR 10.0.0.0/16
3. Note VPC ID (starts with vpc-)

**Review Subnets:**
1. Go to **"Subnets"**
2. You should see 4 subnets:
   - **Public subnet 1:** 10.0.0.0/20 (AZ a) - 4096 IPs
   - **Public subnet 2:** 10.0.16.0/20 (AZ b) - 4096 IPs
   - **Private subnet 1:** 10.0.128.0/20 (AZ a) - 4096 IPs
   - **Private subnet 2:** 10.0.144.0/20 (AZ b) - 4096 IPs

**Review Internet Gateway:**
1. Go to **"Internet Gateways"**
2. Verify IGW created and attached to your VPC
3. Note IGW ID (starts with igw-)

**Review Route Tables:**
1. Go to **"Route Tables"**
2. You should see:
   - **Public route table:**
     - Local route: 10.0.0.0/16 → local
     - Internet route: 0.0.0.0/0 → igw-xxx
     - Associated with public subnets
   - **Private route tables:**
     - Local route only: 10.0.0.0/16 → local
     - Associated with private subnets

#### Part 3: Create Security Groups

**Create Web Server Security Group:**

1. Go to **"Security Groups"**
2. Click **"Create security group"**
3. **Security group name:** "WebServer-SG"
4. **Description:** "Allow HTTP and SSH"
5. **VPC:** Select your VPC (MyVPC)
6. **Inbound rules:**
   - Click **"Add rule"**
     - Type: SSH
     - Source: My IP
   - Click **"Add rule"**
     - Type: HTTP
     - Source: 0.0.0.0/0 (anywhere IPv4)
7. **Outbound rules:** Leave default (allows all outbound)
8. **Tags:**
   - Key: Name
   - Value: WebServer-SG
9. Click **"Create security group"**

**Create Database Security Group:**

1. Click **"Create security group"**
2. **Security group name:** "Database-SG"
3. **Description:** "Allow MySQL from WebServer"
4. **VPC:** Select your VPC (MyVPC)
5. **Inbound rules:**
   - Click **"Add rule"**
     - Type: MySQL/Aurora
     - Port: 3306
     - Source: Custom
     - Search and select "WebServer-SG"
6. **Outbound rules:** Leave default
7. Click **"Create security group"**

> **Explanation:** Database-SG only allows MySQL connections from instances with WebServer-SG, implementing principle of least privilege.

#### Part 4: Launch EC2 Instance in Custom VPC

1. Go to **EC2** service
2. Click **"Launch instance"**
3. **Name:** "VPC-Test-Instance"
4. **AMI:** Amazon Linux 2023 AMI (Free Tier)
5. **Instance type:** t2.micro
6. **Key pair:** Use existing or create new
7. **Network settings:**
   - Click **"Edit"**
   - **VPC:** Select MyVPC
   - **Subnet:** Select public subnet (10.0.0.0/20 or 10.0.16.0/20)
   - **Auto-assign public IP:** Enable
   - **Firewall:** Select existing security group
   - Select **WebServer-SG**
8. Keep other defaults
9. Click **"Launch instance"**
10. Wait for instance to reach "Running" state
11. **Verify:** Instance has public IP and you can SSH into it

#### Part 5: Create Network ACL (NACL)

1. Go to **"Network ACLs"** in VPC
2. Click **"Create network ACL"**
3. **Name:** "Custom-NACL"
4. **VPC:** Select MyVPC
5. Click **"Create network ACL"**

**Configure Inbound Rules:**

1. Select your NACL
2. Go to **"Inbound rules"** tab
3. Click **"Edit inbound rules"**
4. Click **"Add new rule"** and add:
   - **Rule 100:**
     - Type: HTTP (80)
     - Source: 0.0.0.0/0
     - Allow
   - **Rule 110:**
     - Type: SSH (22)
     - Source: 0.0.0.0/0
     - Allow
   - **Rule 120:**
     - Type: Custom TCP
     - Port range: 1024-65535 (ephemeral ports)
     - Source: 0.0.0.0/0
     - Allow
   - **Rule \* (default):** All traffic, Deny
5. Click **"Save changes"**

**Configure Outbound Rules:**

1. Go to **"Outbound rules"** tab
2. Click **"Edit outbound rules"**
3. Add rules:
   - **Rule 100:**
     - Type: HTTP (80)
     - Destination: 0.0.0.0/0
     - Allow
   - **Rule 110:**
     - Type: HTTPS (443)
     - Destination: 0.0.0.0/0
     - Allow
   - **Rule 120:**
     - Type: Custom TCP
     - Port range: 1024-65535
     - Destination: 0.0.0.0/0
     - Allow
4. Click **"Save changes"**

**Associate NACL with Subnet (Optional):**

1. Go to **"Subnet associations"** tab
2. Click **"Edit subnet associations"**
3. Select a public subnet
4. Click **"Save changes"**

> **Note:** Default NACL allows all traffic. Custom NACLs deny all traffic by default.

### Expected Outcomes

- Custom VPC created with CIDR 10.0.0.0/16
- 2 public subnets and 2 private subnets across 2 AZs
- Internet Gateway attached and routes configured
- Security groups created for web server and database
- EC2 instance launched in public subnet
- Custom NACL created and configured

### Verification

1. **VPC exists:** Shows in "Your VPCs" with correct CIDR
2. **Subnets created:** 4 subnets visible with correct CIDR blocks
3. **Routing works:** EC2 instance in public subnet has internet access
4. **Security groups work:** Can SSH to instance, HTTP accessible
5. **NACLs configured:** Rules visible in NACL

### Troubleshooting

**Problem:** Can't create VPC
**Solution:**
- Check you haven't exceeded VPC limit (5 per region default)
- Verify CIDR block doesn't overlap with existing VPCs

**Problem:** EC2 instance has no internet access
**Solution:**
- Verify instance in public subnet
- Check route table has route to IGW (0.0.0.0/0 → igw-xxx)
- Ensure auto-assign public IP enabled
- Verify NACL allows traffic

**Problem:** Can't SSH to instance
**Solution:**
- Check security group allows SSH from your IP
- Verify NACL allows SSH and ephemeral ports
- Ensure instance has public IP
- Check route table configuration

### Cleanup

**Delete resources in order:**

1. **Terminate EC2 instance:**
   - Go to EC2 → Instances
   - Select instance → Terminate

2. **Delete custom security groups:**
   - Go to VPC → Security Groups
   - Select custom SGs → Delete
   - Note: Default SG cannot be deleted

3. **Delete custom NACLs:**
   - Go to VPC → Network ACLs
   - Disassociate from subnets first
   - Select custom NACL → Delete
   - Note: Default NACL cannot be deleted

4. **Delete VPC:**
   - Go to VPC → Your VPCs
   - Select your VPC → Delete VPC
   - This will delete:
     - Subnets
     - Route tables (except default)
     - Internet Gateway
     - VPC itself
   - Confirm deletion

---

## Lab 6: Amazon RDS Database

**Duration:** 30 minutes
**Cost:** Free (db.t3.micro or db.t4g.micro in Free Tier)
**Difficulty:** Intermediate

### Objective

Launch a managed MySQL database using Amazon RDS.

### Prerequisites

- Basic understanding of relational databases
- Completed Lab 3 (EC2 knowledge) and Lab 5 (VPC knowledge)

### Step-by-Step Instructions

#### Part 1: Create RDS Database

1. Navigate to **RDS** service
2. Click **"Create database"**
3. **Database creation method:** Standard create
4. **Engine options:**
   - Engine type: MySQL
   - Version: MySQL 8.0.xx (latest)
5. **Templates:** **Free tier**
   - **Important:** This automatically configures Free Tier eligible options
6. **Settings:**
   - **DB instance identifier:** "mydatabase"
   - **Master username:** admin
   - **Credentials management:** Self managed
   - **Master password:** Create secure password (minimum 8 characters)
   - **Confirm password:** Re-enter password
   - **Save password securely!**

#### Part 2: Configure Instance

1. **DB instance class:**
   - Burstable classes (includes t classes)
   - db.t3.micro or db.t4g.micro (Free Tier eligible)
2. **Storage:**
   - Storage type: General Purpose SSD (gp3)
   - Allocated storage: 20 GiB
   - **Uncheck** "Enable storage autoscaling" (to control costs)
3. **Storage autoscaling:** Disabled

#### Part 3: Configure Connectivity

1. **Compute resource:**
   - Don't connect to an EC2 compute resource
2. **Network type:** IPv4
3. **Virtual private cloud (VPC):**
   - Select Default VPC (or custom VPC if you have one)
4. **DB subnet group:** Default
5. **Public access:** **No** (recommended for security)
   - Database won't have public IP
   - Only accessible from EC2 in same VPC
6. **VPC security group:**
   - Choose existing
   - Create new
   - **Name:** "rds-mysql-sg"
7. **Availability Zone:** No preference
8. **Database port:** 3306 (default)

#### Part 4: Additional Configuration

1. Expand **"Additional configuration"**
2. **Database options:**
   - **Initial database name:** "mydb"
   - This creates a database automatically
3. **Backup:**
   - **Uncheck** "Enable automated backups" (to stay in Free Tier)
   - Free Tier allows backups, but to be safe disable
4. **Encryption:**
   - **Uncheck** "Enable encryption" (optional, for simplicity)
   - In production, always enable encryption
5. **Monitoring:**
   - **Uncheck** "Enable Enhanced monitoring" (to avoid charges)
6. **Maintenance:**
   - Keep defaults
7. Click **"Create database"**
8. Wait 5-10 minutes for database creation
9. **Status** will change: Creating → Backing up → Available

#### Part 5: Review Database Details

1. Once status is **"Available"**, click on database name
2. **Connectivity & security** tab:
   - Note **Endpoint** (e.g., mydatabase.xxxxx.us-east-1.rds.amazonaws.com)
   - Note **Port:** 3306
   - **Security group:** Click to view rules
3. **Configuration** tab:
   - Verify DB instance class, storage, and version

#### Part 6: Connect to RDS (Requires EC2 in Same VPC)

**Launch EC2 instance (if you don't have one):**

1. Go to EC2 → Launch instance
2. Use same VPC as RDS
3. Select public subnet
4. Use Amazon Linux 2023 AMI
5. Launch and SSH into instance

**Install MySQL client on EC2:**

```bash
sudo yum update -y
sudo yum install -y mariadb105
```

**Update RDS Security Group:**

1. Go to VPC → Security Groups
2. Select RDS security group (rds-mysql-sg)
3. Edit inbound rules
4. Add rule:
   - Type: MySQL/Aurora
   - Port: 3306
   - Source: Security group of your EC2 instance
5. Save rules

**Connect to RDS from EC2:**

```bash
mysql -h mydatabase.xxxxx.us-east-1.rds.amazonaws.com -u admin -p
```

Replace with your actual RDS endpoint.

Enter password when prompted.

**Test database:**

```sql
SHOW DATABASES;
USE mydb;
CREATE TABLE users (id INT, name VARCHAR(50));
INSERT INTO users VALUES (1, 'John Doe');
INSERT INTO users VALUES (2, 'Jane Smith');
SELECT * FROM users;
```

Expected output:
```
+------+------------+
| id   | name       |
+------+------------+
|    1 | John Doe   |
|    2 | Jane Smith |
+------+------------+
```

**Exit MySQL:**
```sql
exit;
```

### Expected Outcomes

- RDS MySQL database created and running
- Database accessible from EC2 instance in same VPC
- Successfully connected and executed SQL commands
- Test table created with sample data

### Verification

1. **RDS shows "Available" status**
2. **Can connect from EC2** using MySQL client
3. **SQL commands execute successfully**
4. **No public access** (more secure configuration)

### Troubleshooting

**Problem:** Can't connect to RDS from EC2
**Solution:**
- Verify both in same VPC
- Check RDS security group allows MySQL (3306) from EC2 security group
- Ensure using correct endpoint and credentials
- Verify RDS status is "Available"

**Problem:** Access denied error
**Solution:**
- Double-check username (admin) and password
- Ensure password entered correctly (case-sensitive)
- Check user exists in database

**Problem:** Connection timeout
**Solution:**
- Verify security group rules
- Check EC2 and RDS in same VPC
- Ensure RDS is not publicly accessible (can't connect from outside VPC)
- Check network ACLs

### Cleanup

> **Critical:** RDS instances incur charges if running beyond Free Tier hours (750 hours/month).

1. Go to **RDS** console
2. Select your database
3. Click **"Actions"** → **"Delete"**
4. Delete options:
   - **Uncheck** "Create final snapshot" (for lab purposes)
   - **Uncheck** "Retain automated backups"
   - **Check** "I acknowledge that upon instance deletion..."
5. Type **"delete me"** to confirm
6. Click **"Delete"**
7. Deletion takes 2-5 minutes
8. Verify database removed from list

---

## Lab 7: CloudWatch Monitoring and Alarms

**Duration:** 25 minutes
**Cost:** Free (within Free Tier limits)
**Difficulty:** Beginner

### Objective

Monitor EC2 instances using CloudWatch metrics and create alarms for notifications.

### Prerequisites

- Running EC2 instance (from Lab 3 or new instance)
- Email address for notifications

### Step-by-Step Instructions

#### Part 1: Launch EC2 Instance (if needed)

1. Launch t2.micro EC2 instance if you don't have one
2. Wait for instance to reach "Running" state
3. Note instance ID

#### Part 2: Explore CloudWatch Metrics

1. Navigate to **CloudWatch** service
2. Click **"All metrics"** in left menu
3. Click **"EC2"**
4. Click **"Per-Instance Metrics"**
5. Search for your instance ID
6. Select metrics:
   - **CPUUtilization**
   - **NetworkIn**
   - **NetworkOut**
7. View graphed metrics
8. Change time range (1 hour, 3 hours, 1 day)
9. Change period (1 minute, 5 minutes)

#### Part 3: Create CloudWatch Alarm

1. Select **"CPUUtilization"** metric (checkbox)
2. Click **"Actions"** → **"Create alarm"**
3. **Metric and conditions:**
   - Metric name: CPUUtilization
   - Statistic: Average
   - Period: 5 minutes
4. **Conditions:**
   - Threshold type: Static
   - Whenever CPUUtilization is: Greater
   - than: **70**
5. Click **"Next"**

#### Part 4: Configure SNS Notification

1. **Alarm state trigger:** In alarm
2. **SNS topic:**
   - Create new topic
   - **Topic name:** "EC2-Alerts"
   - **Email endpoints:** Enter your email address
3. Click **"Create topic"**
4. Click **"Next"**
5. **Alarm name:** "High-CPU-Alert"
6. **Alarm description:** "Alert when EC2 CPU exceeds 70%"
7. Click **"Next"**
8. Review settings
9. Click **"Create alarm"**
10. **Check email** and click confirmation link in SNS subscription email

#### Part 5: Test Alarm (Optional)

> **Warning:** This will stress your CPU. Only do if you want to test.

**SSH into EC2 instance:**

```bash
ssh -i your-key.pem ec2-user@[public-ip]
```

**Install stress tool:**

```bash
sudo yum install -y stress
```

**Run CPU stress test:**

```bash
stress --cpu 2 --timeout 300
```

This runs for 5 minutes (300 seconds).

**Monitor alarm:**

1. Go to CloudWatch → Alarms
2. Wait 5-10 minutes
3. Alarm state will change: OK → In alarm
4. You'll receive email notification
5. After stress test completes, alarm returns to OK state

#### Part 6: View CloudWatch Logs

1. In CloudWatch, go to **"Logs"** → **"Log groups"**
2. Click **"Create log group"**
3. **Log group name:** "/aws/my-application"
4. Click **"Create"**
5. Click on log group to explore
6. Note: No logs yet (need to configure application to send logs)

**Explore existing log groups:**
- Look for /aws/lambda/, /aws/rds/, etc.
- Click on log group → Log streams → View logs

### Expected Outcomes

- CloudWatch metrics visible for EC2 instance
- CPU alarm created with 70% threshold
- SNS topic created and email subscription confirmed
- Email notification received when alarm triggered (if tested)
- Log group created

### Verification

1. **Alarm visible** in CloudWatch → Alarms
2. **SNS subscription confirmed** (check email)
3. **Metrics displaying** in graphs
4. **Alarm triggers correctly** (if stress test performed)

### Troubleshooting

**Problem:** No metrics showing for EC2
**Solution:**
- Wait 5-10 minutes after instance launch
- Verify instance is running
- Check correct region selected
- Refresh page

**Problem:** Email notification not received
**Solution:**
- Check spam/junk folder
- Verify email address entered correctly
- Resend confirmation from SNS console
- Check SNS subscription status (should be "Confirmed")

**Problem:** Alarm not triggering
**Solution:**
- Verify CPU is actually exceeding 70%
- Check alarm configuration and threshold
- Wait for evaluation period (5 minutes)
- Review alarm history in details

### Cleanup

1. **Delete CloudWatch alarm:**
   - Go to CloudWatch → Alarms
   - Select alarm → Actions → Delete
   - Confirm deletion

2. **Delete SNS topic:**
   - Go to SNS → Topics
   - Select topic → Delete
   - Type "delete me"
   - Confirm deletion

3. **Delete log group:**
   - Go to CloudWatch → Log groups
   - Select log group → Actions → Delete
   - Confirm deletion

4. **Terminate EC2 instance:**
   - Go to EC2 → Instances
   - Select instance → Instance state → Terminate

---

## Lab 8: AWS Cost Management Tools

**Duration:** 30 minutes
**Cost:** Free
**Difficulty:** Beginner

### Objective

Explore AWS billing and cost management tools including Pricing Calculator, Cost Explorer, Budgets, and Trusted Advisor.

### Prerequisites

- AWS account with some usage (even minimal)
- Billing access enabled

### Step-by-Step Instructions

#### Part 1: AWS Pricing Calculator

1. Open browser and visit [https://calculator.aws](https://calculator.aws)
2. Click **"Create estimate"**
3. **Add EC2:**
   - Search for "EC2"
   - Click **"Configure"**
   - **Region:** Select us-east-1
   - **Quick estimate:**
     - Number of instances: 10
     - Instance type: t3.medium
   - **Pricing model:** On-Demand
   - Review monthly cost estimate
   - Click **"Add to my estimate"**

4. **Add S3:**
   - Search for "S3"
   - Click **"Configure"**
   - **S3 Standard storage:** 1000 GB
   - **PUT/COPY/POST requests:** 100,000
   - **GET requests:** 1,000,000
   - Review cost
   - Click **"Add to my estimate"**

5. **Add RDS:**
   - Search for "RDS"
   - Click **"Configure"**
   - **Database engine:** MySQL
   - **Instance type:** db.t3.medium
   - **Deployment:** Single-AZ
   - **Storage:** 100 GB
   - **Pricing model:** On-Demand
   - Click **"Add to my estimate"**

6. **Review total:**
   - See estimated monthly cost
   - Compare different pricing models:
     - On-Demand
     - Reserved Instances (1-year, 3-year)
     - Savings Plans
   - Note potential savings

7. **Export estimate:**
   - Click **"Export"** → **"PDF"** or **"CSV"**
   - Save for reference

8. **Share estimate:**
   - Click **"Share"**
   - Copy shareable link
   - Can send to colleagues or save for later

#### Part 2: AWS Cost Explorer

> **Note:** Cost Explorer takes 24 hours to populate for new accounts.

1. Go to **Billing and Cost Management** console
2. Click **"Cost Explorer"** in left menu
3. Click **"Launch Cost Explorer"** (if first time)
4. Wait for initialization (if new account, data appears in 24 hours)

**Explore costs (if data available):**

5. **View monthly costs:**
   - Default view shows last 6 months
   - View costs by service
   - Identify top services

6. **Filter by service:**
   - Click filter dropdown
   - Select specific service (EC2, S3, etc.)
   - View service-specific costs

7. **Group by:**
   - Service
   - Region
   - Tag
   - Instance type

8. **View forecast:**
   - See projected costs for next month
   - Based on current usage patterns

9. **Create custom report:**
   - Select date range
   - Choose groupings and filters
   - Click **"Save to report library"**
   - Name: "Monthly Service Breakdown"
   - Save report for future use

10. **Download CSV:**
    - Click **"Download CSV"**
    - Open in spreadsheet for analysis

#### Part 3: Review Bills

1. Go to **"Bills"** in Billing console
2. **View current month charges:**
   - Expand services to see breakdown
   - View charges by region
   - Check data transfer costs
3. **Check Free Tier usage:**
   - Click **"Free Tier"** in left menu
   - View current month usage vs. Free Tier limits
   - **Important:** Monitor to avoid charges
   - See warnings for services approaching limits
4. **Download bill:**
   - Click **"Download CSV"**
   - Save for records

#### Part 4: AWS Budgets

1. Go to **"Budgets"** in Billing console
2. Review budget created in Lab 1 (if you did it)
3. **Create additional budget:**
   - Click **"Create budget"**
   - **Budget type:** Usage budget
   - **Service:** Amazon Elastic Compute Cloud
   - Click **"Next"**

4. **Set budget details:**
   - **Budget name:** "EC2-Usage-Budget"
   - **Period:** Monthly
   - **Usage type:** Running Hours
   - **Unit:** Hrs
   - **Amount:** 750 (Free Tier limit)
   - Click **"Next"**

5. **Configure alert:**
   - **Threshold:** 80% of budgeted amount
   - **Email recipients:** Your email
   - Click **"Add alert threshold"**
   - **Second threshold:** 100%
   - Click **"Next"**

6. Review and click **"Create budget"**

7. **View budgets:**
   - See all budgets in dashboard
   - Monitor current usage vs. budget
   - View alerts history

#### Part 5: AWS Trusted Advisor

1. Navigate to **Trusted Advisor** service
2. **Dashboard overview:**
   - View checks by category:
     - Cost Optimization
     - Performance
     - Security
     - Fault Tolerance
     - Service Limits

3. **Review 7 core checks** (available on Basic support):
   - **S3 Bucket Permissions**
     - Checks for publicly accessible buckets
     - Click to view details
   - **Security Groups - Specific Ports Unrestricted**
     - Identifies overly permissive rules
   - **IAM Use**
     - Checks if you're using IAM
   - **MFA on Root Account**
     - Verifies MFA enabled
   - **EBS Public Snapshots**
     - Checks for public snapshots
   - **RDS Public Snapshots**
     - Checks for public snapshots
   - **Service Limits**
     - Shows usage vs. limits

4. **Click on each check:**
   - View details and recommendations
   - Take action on warnings
   - Green = good, Yellow = investigate, Red = action needed

5. **Refresh checks:**
   - Click **"Refresh all"**
   - Checks update (may take few minutes)

> **Note:** Full Trusted Advisor features require Business or Enterprise support plan.

### Expected Outcomes

- Created cost estimate in Pricing Calculator
- Explored Cost Explorer (if data available)
- Reviewed current billing and Free Tier usage
- Created usage budget for EC2
- Reviewed Trusted Advisor recommendations

### Verification

1. **Pricing estimate created** and can be shared
2. **Cost Explorer launched** (data may take 24 hours)
3. **Current bill viewable** with service breakdown
4. **Budgets configured** with email alerts
5. **Trusted Advisor checks reviewed**

### Troubleshooting

**Problem:** Can't access billing information
**Solution:**
- Enable IAM access to billing in Account settings
- Sign in as root user or IAM user with billing permissions

**Problem:** Cost Explorer shows no data
**Solution:**
- Wait 24 hours after account creation
- Ensure you have some usage (launch services)
- Refresh page

**Problem:** Trusted Advisor shows limited checks
**Solution:**
- Basic support only includes 7 core checks
- Upgrade to Business/Enterprise for full checks
- This is expected behavior

### Cleanup

> **Note:** Keep budgets and Trusted Advisor checks active for ongoing protection. No cleanup needed.

---

## Lab 9: Lambda Serverless Function

**Duration:** 25 minutes
**Cost:** Free (1 million requests/month in Free Tier)
**Difficulty:** Intermediate

### Objective

Create a serverless Lambda function with API Gateway trigger.

### Prerequisites

- Basic programming knowledge (Python helpful but not required)
- Understanding of API concepts

### Step-by-Step Instructions

#### Part 1: Create Lambda Function

1. Navigate to **Lambda** service
2. Click **"Create function"**
3. **Function option:** Author from scratch
4. **Function name:** "HelloWorldFunction"
5. **Runtime:** Python 3.12 (or latest available)
6. **Architecture:** x86_64
7. **Permissions:**
   - Execution role: Create a new role with basic Lambda permissions
   - Role name: (auto-generated)
8. Click **"Create function"**
9. Wait for function creation

#### Part 2: Write Function Code

1. In **Code source** section:
2. Delete existing code in `lambda_function.py`
3. Paste the following code:

```python
import json

def lambda_handler(event, context):
    # Get name from event, default to 'World'
    name = event.get('name', 'World')

    # Create response
    message = f'Hello, {name}!'

    return {
        'statusCode': 200,
        'body': json.dumps(message),
        'headers': {
            'Content-Type': 'application/json'
        }
    }
```

4. Click **"Deploy"** (important!)
5. Wait for "Successfully deployed" message

#### Part 3: Test Function

1. Click **"Test"** button
2. **Configure test event:**
   - **Event name:** "TestEvent"
   - **Event JSON:**
   ```json
   {
     "name": "AWS Student"
   }
   ```
3. Click **"Save"**
4. Click **"Test"** again
5. **View execution results:**
   - **Status:** Succeeded
   - **Response:**
   ```json
   {
     "statusCode": 200,
     "body": "\"Hello, AWS Student!\"",
     "headers": {
       "Content-Type": "application/json"
     }
   }
   ```
6. View **logs** in output
7. Note execution time and memory used

#### Part 4: View CloudWatch Logs

1. Click **"Monitor"** tab
2. Click **"View CloudWatch logs"**
3. Click on latest log stream
4. View log details:
   - START RequestId
   - Function output
   - END RequestId
   - REPORT (duration, memory)

#### Part 5: Configure API Gateway Trigger

1. Go back to **Lambda function** (Code tab)
2. Click **"Add trigger"**
3. **Select a trigger:** API Gateway
4. **API type:** HTTP API
5. **Security:** Open
   - **Warning:** This makes API publicly accessible
   - For production, use authentication
6. Click **"Add"**
7. Wait for trigger creation

#### Part 6: Test API Endpoint

1. In **Configuration** → **Triggers**, click on API Gateway
2. **Copy API endpoint URL**
   - Example: `https://abc123.execute-api.us-east-1.amazonaws.com/default/HelloWorldFunction`
3. **Test in browser:**
   - Paste URL in browser
   - Add query parameter: `?name=YourName`
   - Full URL: `https://abc123.execute-api.us-east-1.amazonaws.com/default/HelloWorldFunction?name=John`
   - **Result:** You should see: `"Hello, John!"`

4. **Test with curl (terminal):**
   ```bash
   curl "https://your-api-url.execute-api.us-east-1.amazonaws.com/default/HelloWorldFunction?name=John"
   ```

5. **Test with different names:**
   - Try `?name=AWS`
   - Try without parameter (should return "Hello, World!")

#### Part 7: Modify Function

1. Go back to **Code** tab
2. Modify code to add more functionality:

```python
import json
from datetime import datetime

def lambda_handler(event, context):
    # Get name from event or query parameters
    name = event.get('name')
    if not name and 'queryStringParameters' in event:
        name = event['queryStringParameters'].get('name', 'World')
    else:
        name = name or 'World'

    # Get current time
    current_time = datetime.now().strftime('%Y-%m-%d %H:%M:%S')

    # Create response
    message = {
        'greeting': f'Hello, {name}!',
        'timestamp': current_time,
        'requestId': context.request_id
    }

    return {
        'statusCode': 200,
        'body': json.dumps(message),
        'headers': {
            'Content-Type': 'application/json'
        }
    }
```

3. Click **"Deploy"**
4. **Test again** with API endpoint
5. Now response includes timestamp and request ID

### Expected Outcomes

- Lambda function created and deployed
- Function executes successfully with test events
- CloudWatch logs capture function output
- API Gateway trigger configured
- Function accessible via public HTTPS endpoint
- Modified function with enhanced functionality

### Verification

1. **Test event executes successfully**
2. **API endpoint returns correct response**
3. **CloudWatch logs show execution details**
4. **Different inputs produce different outputs**

### Troubleshooting

**Problem:** Function fails with syntax error
**Solution:**
- Check Python indentation (use spaces, not tabs)
- Verify all quotes and brackets match
- Review error in CloudWatch logs

**Problem:** API returns "Internal Server Error"
**Solution:**
- Check CloudWatch logs for error details
- Verify function deployed after code changes
- Ensure JSON formatting correct in response

**Problem:** Can't access API endpoint
**Solution:**
- Verify API Gateway trigger added
- Check security set to "Open"
- Ensure using correct HTTP method (GET)
- Try in different browser or incognito mode

**Problem:** Query parameters not working
**Solution:**
- Use modified code that checks queryStringParameters
- Format URL correctly: `?name=Value`
- Check API Gateway integration settings

### Cleanup

1. **Delete Lambda function:**
   - Select function
   - **Actions** → **Delete**
   - Type "delete"
   - Confirm deletion

2. **Delete API Gateway:**
   - Go to **API Gateway** service
   - Select your API
   - **Actions** → **Delete**
   - Confirm deletion

> **Note:** CloudWatch logs persist after function deletion. Delete log group if desired:
> - CloudWatch → Log groups → Select /aws/lambda/HelloWorldFunction → Delete

---

## Lab 10: CloudFormation Infrastructure as Code

**Duration:** 20 minutes
**Cost:** Free (resources created are Free Tier eligible)
**Difficulty:** Intermediate

### Objective

Deploy AWS infrastructure using CloudFormation templates (Infrastructure as Code).

### Prerequisites

- Understanding of YAML or JSON
- Text editor
- Familiarity with S3 (from Lab 4)

### Step-by-Step Instructions

#### Part 1: Create CloudFormation Template

1. Open text editor
2. Create file named `simple-stack.yaml`
3. Paste the following template:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Simple S3 bucket stack for learning CloudFormation

Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'cf-bucket-${AWS::AccountId}'
      VersioningConfiguration:
        Status: Enabled
      Tags:
        - Key: Environment
          Value: Learning
        - Key: ManagedBy
          Value: CloudFormation

Outputs:
  BucketName:
    Description: Name of the S3 bucket
    Value: !Ref MyS3Bucket
  BucketArn:
    Description: ARN of the S3 bucket
    Value: !GetAtt MyS3Bucket.Arn
```

4. **Save file** to your computer

> **Explanation:**
> - **Resources:** Defines S3 bucket with versioning
> - **!Sub:** Substitutes account ID to make bucket name unique
> - **Outputs:** Returns bucket name and ARN after creation

#### Part 2: Create CloudFormation Stack

1. Navigate to **CloudFormation** service
2. Click **"Create stack"** → **"With new resources (standard)"**
3. **Prepare template:** Template is ready
4. **Template source:** Upload a template file
5. Click **"Choose file"** and select `simple-stack.yaml`
6. Click **"Next"**
7. **Stack name:** "MyFirstStack"
8. Click **"Next"**
9. **Configure stack options:**
   - **Tags (optional):**
     - Key: Project
     - Value: CloudFormation-Lab
10. Click **"Next"**
11. **Review:**
    - Verify all settings
    - Review template in JSON/YAML view
12. Click **"Submit"**

#### Part 3: Monitor Stack Creation

1. **Stack status:** CREATE_IN_PROGRESS
2. Click **"Events"** tab
   - Watch real-time creation events
   - See each resource being created
3. Click **"Resources"** tab
   - View logical ID and physical ID
   - See S3 bucket being created
4. Wait for **Status:** CREATE_COMPLETE (takes 1-2 minutes)
5. Click **"Outputs"** tab
   - View BucketName and BucketArn
   - Copy bucket name

#### Part 4: Verify Resource Creation

1. Open new tab → Navigate to **S3** service
2. **Verify bucket exists:**
   - Find bucket: `cf-bucket-[your-account-id]`
   - Click on bucket
   - Verify versioning enabled (Properties → Bucket Versioning)
   - Check tags (Properties → Tags)
3. **Note:** This bucket was created entirely by CloudFormation template

#### Part 5: Update Stack

**Create updated template:**

1. Open `simple-stack.yaml`
2. Add encryption configuration:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Simple S3 bucket stack for learning CloudFormation

Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'cf-bucket-${AWS::AccountId}'
      VersioningConfiguration:
        Status: Enabled
      BucketEncryption:
        ServerSideEncryptionConfiguration:
          - ServerSideEncryptionByDefault:
              SSEAlgorithm: AES256
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true
      Tags:
        - Key: Environment
          Value: Learning
        - Key: ManagedBy
          Value: CloudFormation

Outputs:
  BucketName:
    Description: Name of the S3 bucket
    Value: !Ref MyS3Bucket
  BucketArn:
    Description: ARN of the S3 bucket
    Value: !GetAtt MyS3Bucket.Arn
```

3. Save file

**Update the stack:**

1. Go back to **CloudFormation** console
2. Select **MyFirstStack**
3. Click **"Update"**
4. **Replace current template:** Upload a template file
5. Choose updated `simple-stack.yaml`
6. Click **"Next"**
7. **Parameters:** (none to change)
8. Click **"Next"**
9. **Review change set:**
   - CloudFormation shows what will change
   - Added: Encryption configuration
   - Added: Public access block
   - **Important:** Shows BEFORE making changes
10. Click **"Next"**
11. Review and click **"Submit"**
12. **Watch update:**
    - Status: UPDATE_IN_PROGRESS
    - Events show modifications
    - Status: UPDATE_COMPLETE

**Verify update:**

1. Go to S3 → Your bucket
2. Properties → Default encryption → Verify enabled
3. Properties → Block public access → Verify all enabled

#### Part 6: View Stack Template

1. In CloudFormation, select stack
2. Click **"Template"** tab
3. **View in Designer:**
   - Click **"View in Application Composer"** or **"View in Designer"**
   - See visual representation of resources
   - Shows relationships between resources
4. **Download template:**
   - View in JSON or YAML format
   - Click "Copy to clipboard" if needed

### Expected Outcomes

- CloudFormation stack created successfully
- S3 bucket deployed with versioning enabled
- Stack updated to add encryption
- Template viewable in designer
- Infrastructure defined as code (repeatable, version-controlled)

### Verification

1. **Stack shows CREATE_COMPLETE status**
2. **S3 bucket exists** with correct configuration
3. **Outputs display** bucket name and ARN
4. **Update successful** with encryption enabled
5. **All resources tagged** with CloudFormation info

### Troubleshooting

**Problem:** Stack creation fails with "Bucket already exists"
**Solution:**
- Bucket names must be globally unique
- Change bucket name in template
- Or delete existing bucket first

**Problem:** Template validation error
**Solution:**
- Check YAML syntax (indentation critical)
- Verify all keys spelled correctly
- Use online YAML validator
- Check CloudFormation documentation for resource properties

**Problem:** Update fails
**Solution:**
- Review change set before confirming
- Some properties can't be updated (require replacement)
- Check Events tab for specific error
- May need to delete and recreate stack

**Problem:** Can't delete stack
**Solution:**
- Ensure S3 bucket is empty first
- CloudFormation can't delete non-empty buckets
- Manually empty bucket, then retry delete

### Cleanup

> **Important:** CloudFormation makes cleanup easy - deletes all resources automatically.

1. Go to **CloudFormation** console
2. Select **MyFirstStack**
3. Click **"Delete"**
4. **Confirm deletion**
5. **Monitor deletion:**
   - Status: DELETE_IN_PROGRESS
   - Events show resources being deleted
   - S3 bucket deleted (if empty)
   - Status: DELETE_COMPLETE (or stack disappears)
6. **Verify in S3:**
   - Go to S3 console
   - Bucket should be gone

> **Note:** If deletion fails, it's usually because S3 bucket not empty. Empty bucket manually and retry.

---

## Additional Practice Recommendations

### Explore More AWS Services

1. **DynamoDB**
   - Create NoSQL table
   - Add items using console
   - Query and scan data
   - Explore indexes

2. **CloudTrail**
   - Enable trail
   - View API call history
   - Search for specific events
   - Understand audit logging

3. **AWS Config**
   - Set up Config
   - Track resource configurations
   - View configuration timeline
   - Create compliance rules

4. **Auto Scaling**
   - Create launch template
   - Set up Auto Scaling group
   - Configure scaling policies
   - Test scale-out/scale-in

5. **Elastic Beanstalk**
   - Deploy sample application
   - Explore managed environment
   - View logs and monitoring
   - Update application

### AWS CLI Practice

**Install AWS CLI:**

1. Follow instructions: [https://aws.amazon.com/cli/](https://aws.amazon.com/cli/)
2. Configure credentials: `aws configure`
3. Run basic commands:
   ```bash
   aws s3 ls
   aws ec2 describe-instances
   aws iam list-users
   ```

### Multi-Region Exploration

1. **Compare regions:**
   - Note service availability differences
   - Check pricing variations
   - Test latency from your location

2. **Practice disaster recovery:**
   - Create resources in multiple regions
   - Understand cross-region replication
   - Learn about global services vs. regional

### Cost Management

1. **Monitor Free Tier Dashboard daily**
2. **Set up multiple budgets** for different services
3. **Review Cost Explorer weekly**
4. **Practice using Pricing Calculator** for different scenarios
5. **Understand billing cycle** and payment methods

### Documentation Habits

1. **Take screenshots** of each step
2. **Create diagrams** of architectures built
3. **Write notes** on lessons learned
4. **Document errors** and solutions
5. **Build your own cheat sheet**

### Advanced Labs (After Basics)

1. **VPC Peering**
2. **Load Balancer with Auto Scaling**
3. **RDS Multi-AZ Deployment**
4. **CloudFront with S3 Origin**
5. **Lambda with SQS and DynamoDB**
6. **CodePipeline for CI/CD**

---

## Final Important Reminders

### Always Clean Up Resources

> **Critical:** Leaving resources running can result in unexpected charges after Free Tier expires.

**Daily checklist:**
- [ ] Terminate all EC2 instances
- [ ] Delete RDS databases
- [ ] Empty and delete S3 buckets
- [ ] Delete CloudFormation stacks
- [ ] Remove unused EBS volumes and snapshots
- [ ] Check billing dashboard

### Monitor Costs

- Check **Free Tier usage** daily
- Review **billing alerts**
- Set up **budgets** for each service
- Download **monthly bills** for records
- Use **Cost Explorer** to track trends

### Security Best Practices

- **Never share** root account credentials
- **Enable MFA** on all accounts
- **Use IAM roles** instead of access keys when possible
- **Follow principle of least privilege**
- **Regularly rotate** credentials
- **Review** security group rules frequently

### Learn More

- **AWS Documentation:** [https://docs.aws.amazon.com](https://docs.aws.amazon.com)
- **AWS Skill Builder:** Free training courses
- **AWS Workshops:** [https://workshops.aws](https://workshops.aws)
- **AWS YouTube:** Official tutorials and demos
- **AWS re:Post:** Community Q&A forum

---

## Congratulations!

You've completed all 10 hands-on labs! You now have practical experience with:

- Billing and cost management
- IAM security
- EC2 compute instances
- S3 object storage
- VPC networking
- RDS managed databases
- CloudWatch monitoring
- Lambda serverless functions
- CloudFormation infrastructure as code

This hands-on knowledge will significantly help you on the AWS Cloud Practitioner exam and in real-world AWS usage.

**Next steps:**
- Review weak areas
- Take practice exams
- Study theory in conjunction with practical experience
- Schedule your certification exam

Good luck on your AWS Cloud Practitioner journey!

---

[← Back to Study Plan](06-study-plan.md) | [Return to Main Guide →](README.md)
