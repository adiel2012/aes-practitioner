# Hands-On Practice Labs

[← Back to Study Plan](06-study-plan.md) | [Return to Main Guide →](README.md)

---

## Table of Contents
- [Introduction](#introduction)
- [Pre-Lab Checklist](#pre-lab-checklist)
- [Prerequisites](#prerequisites)
- [Important Notes](#important-notes)
- [Lab Difficulty Guide](#lab-difficulty-guide)
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
- [Lab 11: Auto Scaling and Load Balancing](#lab-11-auto-scaling-and-load-balancing)
- [Lab 12: DynamoDB Hands-On](#lab-12-dynamodb-hands-on)
- [Lab 13: SNS and SQS Messaging](#lab-13-sns-and-sqs-messaging)
- [Lab 14: Route 53 DNS and Health Checks](#lab-14-route-53-dns-and-health-checks)
- [Lab 15: AWS Organizations and Multi-Account Setup](#lab-15-aws-organizations-and-multi-account-setup)
- [Troubleshooting FAQ](#troubleshooting-faq)
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

**Total Time Investment:** Approximately 11-12 hours for all 15 labs

---

## Pre-Lab Checklist

Before starting any lab, ensure you have completed the following:

### Essential Requirements
- [ ] **AWS Account created and activated** (may take up to 24 hours)
- [ ] **Valid credit/debit card** on file (required even for Free Tier)
- [ ] **Email access** to confirm SNS subscriptions and receive alerts
- [ ] **Billing alerts set up** (Lab 1 - do this FIRST!)
- [ ] **Free Tier dashboard bookmarked** for easy monitoring
- [ ] **Account ID noted** and saved securely

### Technical Setup
- [ ] **Modern web browser** (Chrome, Firefox, Safari, Edge - latest version)
- [ ] **Stable internet connection** (minimum 5 Mbps recommended)
- [ ] **Text editor** installed (VS Code, Sublime, Notepad++, or any editor)
- [ ] **SSH client available** (built-in on Mac/Linux, PuTTY or OpenSSH for Windows)
- [ ] **Terminal/command prompt** access and basic familiarity

### Knowledge Prerequisites
- [ ] **Basic understanding** of cloud computing concepts
- [ ] **Familiarity with IP addresses** and networking basics (for VPC labs)
- [ ] **Basic command line** experience (helpful but not required)
- [ ] **Understanding of JSON/YAML** formats (for CloudFormation)

### Safety Measures
- [ ] **Password manager** or secure location for credentials
- [ ] **Notepad ready** for documenting resource IDs and URLs
- [ ] **Calendar reminder** set to check and clean up resources daily
- [ ] **Budget limit decided** (recommend $10-20 maximum)
- [ ] **Understanding of charges** - know what costs money vs. what's free

### Best Practices Before Starting
- [ ] **Read entire lab** before executing any steps
- [ ] **Take screenshots** as you progress for documentation
- [ ] **Use consistent naming** conventions (include date or lab number)
- [ ] **Tag all resources** with project name for easy identification
- [ ] **Set aside uninterrupted time** for each lab
- [ ] **Prepare to take notes** on errors and solutions

### Region Selection
- [ ] **Choose your primary region** (recommend us-east-1 or us-west-2)
- [ ] **Verify Free Tier availability** in chosen region
- [ ] **Note region for all labs** to maintain consistency
- [ ] **Bookmark region selector** for quick access

### Time Planning
- [ ] **Review lab duration** estimates
- [ ] **Add 25% buffer time** for troubleshooting
- [ ] **Plan cleanup time** (5-10 minutes per lab)
- [ ] **Schedule breaks** between complex labs
- [ ] **Avoid starting labs** late at night (may forget cleanup)

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

## Lab Difficulty Guide

### Understanding Difficulty Ratings

Each lab is rated on three dimensions:

**Technical Complexity:**
- **Beginner:** Straightforward steps, minimal technical knowledge required
- **Intermediate:** Some technical concepts, may require troubleshooting
- **Advanced:** Complex networking/architecture, requires careful attention

**Time Investment:**
- Short: 15-30 minutes
- Medium: 30-60 minutes
- Long: 60+ minutes

**Prerequisites:**
- Minimal: Just AWS account
- Moderate: Completion of earlier labs helpful
- Extensive: Requires specific knowledge or completed labs

### Lab Difficulty Matrix

| Lab | Difficulty | Duration | Prerequisites | Complexity Score |
|-----|-----------|----------|---------------|------------------|
| Lab 1: Billing Alerts | Beginner | 15 min | None | 1/5 |
| Lab 2: IAM & MFA | Beginner | 30 min | None | 2/5 |
| Lab 3: EC2 Instance | Intermediate | 45 min | Lab 2 recommended | 3/5 |
| Lab 4: S3 Website | Intermediate | 40 min | Basic HTML | 2/5 |
| Lab 5: VPC Network | Advanced | 60 min | Networking basics | 4/5 |
| Lab 6: RDS Database | Intermediate | 30 min | Lab 3 & 5 | 3/5 |
| Lab 7: CloudWatch | Beginner | 25 min | Lab 3 | 2/5 |
| Lab 8: Cost Tools | Beginner | 30 min | None | 1/5 |
| Lab 9: Lambda | Intermediate | 25 min | Basic Python | 2/5 |
| Lab 10: CloudFormation | Intermediate | 20 min | YAML/JSON | 3/5 |
| Lab 11: Auto Scaling | Advanced | 45 min | Lab 3 & 5 | 4/5 |
| Lab 12: DynamoDB | Intermediate | 35 min | Database concepts | 2/5 |
| Lab 13: SNS & SQS | Intermediate | 30 min | Lab 9 helpful | 3/5 |
| Lab 14: Route 53 | Intermediate | 25 min | DNS knowledge | 3/5 |
| Lab 15: Organizations | Advanced | 40 min | Multiple accounts | 4/5 |

### Recommended Learning Paths

**Path 1: Absolute Beginners**
1. Lab 1 (Billing) → Lab 2 (IAM) → Lab 8 (Cost Tools) → Lab 4 (S3)
2. Then proceed with: Lab 3 → Lab 7 → Lab 9 → Lab 12

**Path 2: Developers**
1. Lab 1 → Lab 2 → Lab 3 → Lab 9 (Lambda)
2. Then: Lab 4 → Lab 13 (Messaging) → Lab 12 (DynamoDB) → Lab 10 (IaC)

**Path 3: Infrastructure/Operations**
1. Lab 1 → Lab 2 → Lab 3 → Lab 5 (VPC)
2. Then: Lab 11 (Auto Scaling) → Lab 6 (RDS) → Lab 14 (Route 53) → Lab 15

**Path 4: Complete Sequential** (Recommended for Most)
- Follow labs 1-15 in order for comprehensive understanding

### Skill Development Tracking

After completing each lab, self-assess your confidence:

**Rating Scale:**
- 1 = Need to review
- 2 = Understood with help
- 3 = Comfortable, could explain to others
- 4 = Expert, could troubleshoot independently
- 5 = Could teach this topic

**Target Scores for Exam:**
- Core labs (1-10): Aim for 4/5
- Advanced labs (11-15): Aim for 3/5

---

## Lab 1: Set Up Billing Alerts and Budget

**Duration:** 15 minutes
**Cost:** Free
**Difficulty:** Beginner

### Learning Objectives

By the end of this lab, you will be able to:

1. Enable IAM user access to billing information
2. Create CloudWatch billing alarms with SNS notifications
3. Configure AWS Budgets with multiple alert thresholds
4. Understand the difference between CloudWatch billing metrics and AWS Budgets
5. Monitor Free Tier usage to prevent unexpected charges
6. Set up proactive cost monitoring for AWS account safety

### Why This Lab Matters

**Real-World Scenario:** A developer left an RDS database running in a personal AWS account and accumulated $450 in charges over a weekend. Proper billing alerts would have caught this within hours, limiting damage to less than $20.

**Exam Relevance:** The Cloud Practitioner exam heavily emphasizes cost management, billing, and monitoring. Questions often test your understanding of:
- AWS Budgets vs. Cost Explorer vs. CloudWatch billing alarms
- Free Tier limits and monitoring
- Billing alert best practices
- SNS notification mechanisms

### Objective

Protect yourself from unexpected charges by setting up billing alerts and budgets.

### Prerequisites

- Active AWS account
- Access to root account or IAM user with billing permissions

### Step-by-Step Instructions

#### Part 1: Enable Billing Alerts

> **What You'll See:** The AWS Management Console with the navigation bar at the top showing your account name and region selector.

1. Sign in to **AWS Management Console** as root user or admin
   - You should see the AWS Services search bar and dashboard

2. Click your **account name** (top right corner) → Select **"Account"**
   - This opens the Account settings page
   - Alternative path: Navigate via "My Account" link in dropdown

3. Scroll down to **"IAM User and Role Access to Billing Information"** section
   - This section is about halfway down the page
   - You'll see a description about allowing IAM users to access billing data
   - **Why this matters:** By default, only root users can see billing info. This setting allows IAM users with appropriate permissions to view costs.

4. Click **"Edit"** button on the right side
   - A popup or inline editor will appear

5. Check the box to **"Activate IAM Access"**
   - When enabled, IAM users with billing permissions can see costs
   - **Best Practice:** This allows you to use IAM users instead of root for daily billing monitoring

6. Click **"Update"**
   - You'll see a success message: "Successfully updated IAM user/role access to billing information"

**Validation Step:**
- The setting should now show as "Activated"
- This is a one-time setup per AWS account

**Common Issue:** If you don't see this option, verify you're logged in as the root user (account owner), not an IAM user.

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

### What You Should See at Each Step

**After Creating CloudWatch Alarm:**
- Alarm appears in CloudWatch → Alarms dashboard
- Status shows "Insufficient data" initially (normal - need 24 hours of data)
- After confirmation, status changes to "OK" (no alarm condition met)
- Graph shows estimated charges over time

**After Confirming SNS Subscription:**
- Email from "AWS Notifications" with subject "AWS Notification - Subscription Confirmation"
- After clicking link, browser shows "Subscription confirmed!"
- SNS console shows subscription status as "Confirmed"

**After Creating Budget:**
- Budget appears in Budgets dashboard
- Shows current spend vs. budgeted amount (likely $0.00 of $10.00)
- Alert thresholds displayed at 80% ($8) and 100% ($10)
- Forecast shows projected end-of-month spend

### Real-World Tips

**Recommended Alert Thresholds:**
- For students/learners: $5 alarm, $10 budget
- For production accounts: Set based on expected monthly spend
- Conservative approach: Alert at 50%, 80%, and 100%

**Multiple Budget Strategy:**
- Total account budget: $10
- Per-service budgets: EC2 $3, RDS $3, S3 $2, Other $2
- Helps identify which service is driving costs

**Alert Fatigue Prevention:**
- Don't set thresholds too low (avoid constant alerts)
- Review and adjust monthly based on actual usage
- Use forecasted budgets for proactive monitoring

**Best Practice - Multiple Notification Recipients:**
- Add team members' emails to SNS topic
- Consider SMS notifications for critical budgets (note: SMS costs money)
- Set up Slack/Teams integration via Lambda (advanced)

### Alternative Approaches

**Method 1: AWS Budgets Only**
- Skip CloudWatch billing alarm
- Use only AWS Budgets (simpler for beginners)
- Limitation: Less granular, updated 3x per day vs. CloudWatch every 5 minutes

**Method 2: CloudWatch Only**
- Skip AWS Budgets
- Create multiple CloudWatch alarms at different thresholds
- Limitation: Doesn't show forecasts or budget tracking UI

**Method 3: Cost Anomaly Detection (Advanced)**
- AWS provides ML-based anomaly detection
- Navigate to Cost Management → Cost Anomaly Detection
- Create monitor → Set alert preferences
- Automatically detects unusual spending patterns

### Cleanup

> **Note:** Keep these alerts active for ongoing protection. No cleanup needed.

**If you need to remove alerts later:**

1. **Delete CloudWatch Alarm:**
   - CloudWatch → Alarms → Select alarm → Actions → Delete
   - Confirm deletion by typing alarm name
   - Alarm deleted immediately

2. **Delete Budget:**
   - Billing → Budgets → Select budget → Actions → Delete budget
   - Type "delete" to confirm
   - Budget removed from dashboard

3. **Delete SNS Topic:**
   - SNS → Topics → Select topic → Delete
   - Type "delete me" to confirm
   - Associated subscriptions automatically deleted

4. **Verify Cleanup:**
   - Check CloudWatch Alarms list (should be empty)
   - Check Budgets dashboard (should show no budgets)
   - Emails stop arriving

**Cost Impact of Deletion:**
- No ongoing costs for these free services
- Safe to keep active indefinitely

---

### Post-Lab Knowledge Check

Test your understanding of Lab 1 concepts:

**Question 1:** What's the difference between CloudWatch billing alarms and AWS Budgets?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
- **CloudWatch Alarms:** Monitor actual charges in near real-time (every 5-10 minutes), trigger SNS notifications when threshold exceeded. Only available in us-east-1 region.
- **AWS Budgets:** Track costs and usage against planned budgets, provide forecasting, update 3 times per day. Support usage-based budgets (not just cost). Available globally.
- **Use both together:** CloudWatch for immediate alerts, Budgets for tracking and forecasting.

</details>

**Question 2:** Why must CloudWatch billing metrics be created in the us-east-1 region?

<details>
<summary>Click to reveal answer</summary>

**Answer:** Billing is a global service, and AWS consolidates all billing data in the us-east-1 (N. Virginia) region. Billing metrics are only published to CloudWatch in this region. This is an AWS design decision to centralize global billing data.

**Exam Tip:** Remember this for the exam - it's a common trick question!

</details>

**Question 3:** If your alarm shows "Insufficient data" status, is something wrong?

<details>
<summary>Click to reveal answer</summary>

**Answer:** No, this is normal. "Insufficient data" means CloudWatch doesn't have enough data points yet to evaluate the alarm. This typically happens:
- Within first 24 hours of alarm creation
- For new AWS accounts with minimal usage
- After changing alarm parameters

Status will change to "OK" once sufficient data is available. If charges exceed threshold, status changes to "In alarm."

</details>

**Question 4:** You set a $10 budget but received an alert at $8. Why?

<details>
<summary>Click to reveal answer</summary>

**Answer:** You configured an alert threshold at 80% of your budget. 80% of $10 = $8. This is intentional and a best practice. Getting alerts before hitting 100% gives you time to investigate and take action before exceeding your budget.

Multiple thresholds (50%, 80%, 100%) provide escalating warnings as you approach your limit.

</details>

**Question 5:** Can you set up billing alarms for individual services like EC2 or S3?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
- **CloudWatch Billing Alarms:** Can only monitor total estimated charges for the account, not individual services.
- **AWS Budgets:** YES, can create service-specific budgets (e.g., EC2 only, S3 only).
- **Best Practice:** Use AWS Budgets for service-level cost tracking, CloudWatch alarms for total account spending.

</details>

**Question 6:** What happens if you don't confirm the SNS subscription email?

<details>
<summary>Click to reveal answer</summary>

**Answer:** The subscription remains in "Pending confirmation" status and you will NOT receive any alarm notifications. The alarm will still evaluate and trigger, but emails won't be sent.

Always check spam folder and confirm subscriptions within 3 days. After 3 days, you may need to recreate the subscription.

</details>

**Question 7:** Your Free Tier includes 10 CloudWatch alarms. What happens if you create 11?

<details>
<summary>Click to reveal answer</summary>

**Answer:** You'll be charged $0.10 per alarm per month for the 11th alarm (and any beyond that). With 11 alarms, cost would be $0.10/month.

**Best Practice for Free Tier:**
- Stay within 10 alarms
- Use AWS Budgets (free) for additional monitoring
- Combine multiple thresholds in fewer alarms

</details>

**Question 8:** How often should you check your Free Tier usage dashboard?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
- **Minimum:** Weekly
- **Recommended:** Every 2-3 days when actively learning
- **Best Practice:** Daily while running labs
- **Set Calendar Reminder:** Add recurring reminder to check dashboard

Free Tier dashboard shows current month usage vs. limits with visual progress bars. Catches issues before they become charges.

</details>

### Key Takeaways

- **Billing alerts are your safety net** - Set them up before doing anything else in AWS
- **Use both CloudWatch alarms and Budgets** - They complement each other
- **Always confirm SNS subscriptions** - Check spam folder if needed
- **Monitor Free Tier usage** - Make it a daily habit during learning phase
- **Be conservative with thresholds** - Better to get warned early than surprised by charges
- **Billing metrics are us-east-1 only** - Remember this for the exam
- **Budgets can track usage, not just costs** - Useful for monitoring Free Tier hours
- **Free Tier is per service** - 750 EC2 hours + 750 RDS hours, not combined

### Additional Resources

- [AWS Billing and Cost Management Documentation](https://docs.aws.amazon.com/account-billing/)
- [CloudWatch Billing Metrics Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html)
- [AWS Budgets Best Practices](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-best-practices.html)
- [Understanding AWS Free Tier](https://aws.amazon.com/free/)

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

## Lab 11: Auto Scaling and Load Balancing

**Duration:** 45 minutes
**Cost:** Free (within Free Tier limits)
**Difficulty:** Advanced

### Learning Objectives

By the end of this lab, you will be able to:

1. Create a Launch Template for EC2 instances
2. Configure an Application Load Balancer (ALB)
3. Set up an Auto Scaling Group with scaling policies
4. Understand target tracking and step scaling
5. Test automatic scale-out and scale-in behaviors
6. Monitor Auto Scaling activities in CloudWatch

### Why This Lab Matters

**Real-World Scenario:** An e-commerce website experiences 10x traffic during Black Friday sales. Auto Scaling automatically adds servers during peak hours and removes them when traffic decreases, optimizing both performance and cost.

**Exam Relevance:** Auto Scaling and ELB are heavily tested topics. Know:
- Types of load balancers (ALB, NLB, CLB)
- Auto Scaling components (launch templates, groups, policies)
- Scaling policies (target tracking, step, scheduled)
- Health checks and high availability

### Prerequisites

- Completed Lab 3 (EC2) and Lab 5 (VPC)
- Understanding of load balancing concepts
- Basic knowledge of web servers

### Step-by-Step Instructions

#### Part 1: Create Launch Template

> **What You'll See:** Launch template configuration wizard with pre-defined settings for EC2 instances.

1. Navigate to **EC2** service
2. In left menu, click **"Launch Templates"**
3. Click **"Create launch template"**

4. **Launch template name:** "WebServer-Template"
5. **Template version description:** "Initial version with Apache"
6. **Auto Scaling guidance:** Check "Provide guidance to help me set up a template that I can use with EC2 Auto Scaling"

7. **Application and OS Images (AMI):**
   - Click **"Quick Start"**
   - Select **"Amazon Linux 2023 AMI"**
   - Verify "Free tier eligible" label

8. **Instance type:** t2.micro (or t3.micro)

9. **Key pair:** Select existing key pair or create new one
   - **Best Practice:** Use existing key from Lab 3 if available

10. **Network settings:**
    - **Subnet:** Don't include in launch template (let Auto Scaling choose)
    - **Firewall (security groups):**
      - Click **"Create security group"**
      - **Name:** "ALB-WebServer-SG"
      - **Description:** "Allow HTTP from Load Balancer"
      - **VPC:** Default VPC
      - **Inbound rules:**
        - Rule 1: HTTP (80), Source: 0.0.0.0/0
        - Rule 2: SSH (22), Source: My IP

11. **Advanced details:**
    - Scroll to **"User data"** section
    - Paste the following script:

```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd

# Create unique web page showing instance ID
INSTANCE_ID=$(ec2-metadata --instance-id | cut -d " " -f 2)
AZ=$(ec2-metadata --availability-zone | cut -d " " -f 2)
cat > /var/www/html/index.html <<EOF
<!DOCTYPE html>
<html>
<head>
    <title>Auto Scaling Demo</title>
    <style>
        body { font-family: Arial; text-align: center; margin-top: 50px; }
        .box { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
               color: white; padding: 40px; border-radius: 10px;
               display: inline-block; }
        h1 { margin: 0; }
        p { font-size: 18px; }
    </style>
</head>
<body>
    <div class="box">
        <h1>Auto Scaling is Working!</h1>
        <p><strong>Instance ID:</strong> $INSTANCE_ID</p>
        <p><strong>Availability Zone:</strong> $AZ</p>
        <p>Refresh to see different instances</p>
    </div>
</body>
</html>
EOF
```

12. Click **"Create launch template"**
13. You'll see success message: "Successfully created WebServer-Template"
14. Click **"View launch template"** to verify

**Validation:**
- Template shows in list with version 1
- All configuration visible in template details

#### Part 2: Create Application Load Balancer

1. In EC2 console, scroll down left menu to **"Load Balancers"**
2. Click **"Create load balancer"**
3. **Load balancer types:** Select **"Application Load Balancer"**
4. Click **"Create"**

5. **Basic configuration:**
   - **Name:** "WebApp-ALB"
   - **Scheme:** Internet-facing
   - **IP address type:** IPv4

6. **Network mapping:**
   - **VPC:** Default VPC
   - **Mappings:** Select at least 2 Availability Zones
     - Check boxes for us-east-1a, us-east-1b (or your region's AZs)
     - Select public subnets for each

7. **Security groups:**
   - Click **"Create new security group"** (opens new tab)
   - **Name:** "ALB-SG"
   - **Description:** "Allow HTTP from internet"
   - **VPC:** Default
   - **Inbound rules:**
     - Type: HTTP, Port: 80, Source: 0.0.0.0/0
   - **Outbound rules:** Keep default (all traffic)
   - Click **"Create security group"**
   - Return to ALB tab, refresh security groups list
   - Select **"ALB-SG"**
   - **Remove default security group**

8. **Listeners and routing:**
   - Protocol: HTTP, Port: 80 (default)
   - **Default action:** Create target group
     - Click **"Create target group"** (opens new tab)

#### Part 3: Create Target Group

1. **Target type:** Instances
2. **Target group name:** "WebApp-TG"
3. **Protocol:** HTTP, Port: 80
4. **VPC:** Default VPC

5. **Health checks:**
   - **Protocol:** HTTP
   - **Path:** / (root path)
   - **Advanced health check settings:**
     - Healthy threshold: 2
     - Unhealthy threshold: 2
     - Timeout: 5 seconds
     - Interval: 30 seconds
     - Success codes: 200
   - **Why these settings:** Fast health checks (30s interval) with quick failover (2 failed checks = unhealthy)

6. Click **"Next"**
7. **Register targets:** Skip (Auto Scaling will register instances automatically)
8. Click **"Create target group"**
9. Return to ALB tab, refresh target groups
10. Select **"WebApp-TG"** from dropdown

11. **Tags (optional):**
    - Key: Project, Value: AutoScaling-Lab

12. **Review** all settings
13. Click **"Create load balancer"**
14. Wait 2-3 minutes for **State:** Active

**Validation:**
- ALB shows "Active" state
- Copy DNS name (e.g., WebApp-ALB-1234567890.us-east-1.elb.amazonaws.com)
- Try accessing in browser (will show 503 error - no targets yet, this is expected)

#### Part 4: Create Auto Scaling Group

1. In EC2 left menu, click **"Auto Scaling Groups"**
2. Click **"Create Auto Scaling group"**

3. **Step 1: Choose launch template**
   - **Name:** "WebApp-ASG"
   - **Launch template:** Select "WebServer-Template"
   - **Version:** Latest (1)
   - Click **"Next"**

4. **Step 2: Choose instance launch options**
   - **VPC:** Default VPC
   - **Availability Zones and subnets:** Select 2 or more AZs
     - Choose public subnets in each AZ
   - Click **"Next"**

5. **Step 3: Configure advanced options**
   - **Load balancing:** Attach to an existing load balancer
   - **Choose from your load balancer target groups**
   - Select **"WebApp-TG"**
   - **Health checks:**
     - Check **"Turn on Elastic Load Balancing health checks"**
     - Health check grace period: 300 seconds
     - **Why:** Gives instances time to fully start before health checks
   - **Monitoring:**
     - Check **"Enable group metrics collection within CloudWatch"**
   - Click **"Next"**

6. **Step 4: Configure group size and scaling**
   - **Group size:**
     - Desired capacity: 2
     - Minimum capacity: 1
     - Maximum capacity: 4
   - **Scaling policies:**
     - Select **"Target tracking scaling policy"**
     - **Scaling policy name:** "Target-Tracking-CPU"
     - **Metric type:** Average CPU utilization
     - **Target value:** 50
     - **Instance warmup:** 300 seconds
     - **Why this works:** When average CPU across all instances exceeds 50%, add instances. When below 50%, remove instances.
   - Click **"Next"**

7. **Step 5: Add notifications (optional)**
   - Skip or add SNS topic for scaling events
   - Click **"Next"**

8. **Step 6: Add tags**
   - **Key:** Name, **Value:** AutoScaled-WebServer
   - Check **"Tag new instances"**
   - Click **"Next"**

9. **Step 7: Review**
   - Verify all settings
   - Click **"Create Auto Scaling group"**

10. **Monitor creation:**
    - ASG created immediately
    - Watch "Activity" tab for instance launches
    - Wait 3-5 minutes for 2 instances to launch and become healthy

**Validation:**
- Auto Scaling group shows "2 instances" desired/running
- Activity history shows successful launches
- Go to Target Group → Targets tab → Both instances show "healthy"
- Access ALB DNS name → See web page with instance ID

#### Part 5: Test Load Balancing

1. **Copy ALB DNS name** from Load Balancers page
2. **Open in browser:** `http://WebApp-ALB-1234567890.us-east-1.elb.amazonaws.com`
3. You should see: "Auto Scaling is Working!" with an instance ID
4. **Refresh page multiple times** (F5 or Cmd+R)
5. Notice instance ID changes between refreshes
   - **What's happening:** ALB distributes requests across instances (round-robin by default)

6. **Test from command line (optional):**
```bash
for i in {1..10}; do curl http://your-alb-dns-name.elb.amazonaws.com | grep "Instance ID"; done
```
   - Shows distribution across instances

**Expected Behavior:**
- Requests alternate between 2 different instance IDs
- Both instances serve traffic
- Response time is fast (<100ms)

#### Part 6: Test Auto Scaling (Scale Out)

> **Warning:** This generates CPU load. Monitor closely and stop if needed.

1. **SSH into one instance:**
   - Go to EC2 → Instances
   - Find instances tagged "AutoScaled-WebServer"
   - Connect via SSH:
   ```bash
   ssh -i your-key.pem ec2-user@[public-ip]
   ```

2. **Install stress tool:**
```bash
sudo yum install -y stress
```

3. **Generate CPU load:**
```bash
stress --cpu 2 --timeout 600
```
   - Runs for 10 minutes (600 seconds)
   - Pushes CPU to ~100%

4. **Monitor Auto Scaling:**
   - Go to Auto Scaling Groups → WebApp-ASG
   - Click **"Activity"** tab
   - Watch for new scaling activities
   - **Time to scale:** 5-10 minutes typically
     - 5 minutes of high CPU (CloudWatch evaluation)
     - 2-3 minutes to launch new instance
     - 5 minutes warmup period

5. **Watch CloudWatch metrics:**
   - Go to CloudWatch → Metrics → EC2 → By Auto Scaling Group
   - Select CPUUtilization for WebApp-ASG
   - **Graph settings:** 1-minute period
   - You'll see CPU spike above 50% target

6. **Verify scale-out:**
   - Auto Scaling group capacity increases: 2 → 3 or 3 → 4
   - Activity history shows: "Launching a new EC2 instance"
   - New instance appears in Instances list
   - Target group shows 3-4 healthy targets

**What You Should See:**
- CPU metric crosses 50% threshold
- After ~5 minutes, scaling activity triggers
- New instance launches automatically
- Total capacity increases
- Load distributes across more instances

#### Part 7: Test Auto Scaling (Scale In)

1. **Stop stress test:** Press Ctrl+C in SSH session (or wait for timeout)
2. **Exit SSH:** Type `exit`

3. **Monitor CPU decrease:**
   - CloudWatch shows CPU dropping below 50%
   - Wait 15-20 minutes for scale-in
   - **Why longer:** AWS conservatively waits before removing capacity

4. **Watch Auto Scaling Activity:**
   - Activity tab shows: "Terminating EC2 instance"
   - Capacity decreases back to 2 (desired capacity)
   - Extra instance terminates automatically

**Expected Timeline:**
- CPU drops: Immediate
- Scale-in evaluation: 15 minutes (default cooldown)
- Instance termination: 2-3 minutes
- Total time to scale in: ~20 minutes

### Expected Outcomes

- Launch template created with user data script
- Application Load Balancer distributing traffic across AZs
- Target group with health checks configured
- Auto Scaling group maintaining 2 instances normally
- Automatic scale-out when CPU exceeds 50%
- Automatic scale-in when CPU returns to normal
- All instances serving traffic through ALB

### Verification Checklist

- [ ] Launch template shows in EC2 templates list
- [ ] ALB status is "Active"
- [ ] Target group shows all instances "healthy"
- [ ] Accessing ALB URL displays web page
- [ ] Refreshing shows different instance IDs (load balancing)
- [ ] Auto Scaling group maintains desired capacity
- [ ] Scale-out occurred during stress test
- [ ] Scale-in occurred after CPU normalized

### Real-World Tips

**Launch Template Best Practices:**
- Version templates for rollback capability
- Use latest Amazon Linux AMI for security patches
- Include monitoring agents in user data
- Test user data scripts before using in templates

**Load Balancer Configuration:**
- Always use at least 2 AZs for high availability
- Configure appropriate health check paths (not just /)
- Set reasonable timeout values (5-10 seconds)
- Use HTTPS in production (requires SSL certificate)
- Enable access logs for troubleshooting

**Auto Scaling Tuning:**
- **Conservative scaling:** Lower thresholds (40% CPU) with longer cooldowns
- **Aggressive scaling:** Higher thresholds (70% CPU) with shorter cooldowns
- **Production recommendation:** Start conservative, tune based on metrics
- **Cost optimization:** Use scheduled scaling for predictable patterns

**Common Scaling Metrics:**
- CPU utilization: Most common, good for compute-bound apps
- Request count per target: Good for web apps with uniform requests
- Network throughput: For network-intensive applications
- Custom CloudWatch metrics: Application-specific (queue length, etc.)

### Troubleshooting

**Problem:** Instances launch but stay "unhealthy" in target group

**Solution:**
- Check security group allows HTTP (port 80) from ALB
- Verify health check path is correct (/)
- Ensure web server started (check user data logs: /var/log/cloud-init-output.log)
- Increase health check grace period to 400-500 seconds
- SSH to instance and test: `curl localhost`

**Problem:** Auto Scaling doesn't scale out despite high CPU

**Solution:**
- Verify CPU metric is publishing to CloudWatch (EC2 → Instances → Monitoring)
- Check Auto Scaling group already at maximum capacity (4 instances)
- Wait full evaluation period (5 minutes of high CPU)
- Review scaling policy configuration and thresholds
- Check CloudWatch Alarms for scaling policy

**Problem:** Cannot access load balancer URL (timeout)

**Solution:**
- Verify ALB security group allows HTTP from 0.0.0.0/0
- Ensure ALB is in public subnets with internet gateway
- Check ALB state is "Active" not "Provisioning"
- Verify at least one target is healthy
- Check route tables have internet gateway route

**Problem:** Scale-in never occurs

**Solution:**
- Default scale-in protection may be enabled (check ASG settings)
- Wait longer (scale-in takes 15-20 minutes)
- Verify CPU actually dropped below threshold
- Check instance protection settings on individual instances
- Review scale-in policies (may have different thresholds)

**Problem:** Web page not showing instance ID

**Solution:**
- User data script may have failed
- SSH to instance: `sudo cat /var/log/cloud-init-output.log`
- Check for script errors
- Verify httpd is running: `sudo systemctl status httpd`
- Test HTML file: `cat /var/www/html/index.html`

### Cleanup

> **Critical:** Load Balancers and running EC2 instances incur charges. Clean up immediately after lab.

**Cleanup Order (Important - follow sequence):**

1. **Delete Auto Scaling Group:**
   - Go to Auto Scaling Groups
   - Select "WebApp-ASG"
   - **Actions** → **Delete**
   - Type "delete" to confirm
   - **This terminates all instances in the group**
   - Wait for instances to terminate (2-3 minutes)

2. **Verify instances terminated:**
   - Go to EC2 → Instances
   - Ensure all "AutoScaled-WebServer" instances show "Terminated"

3. **Delete Load Balancer:**
   - Go to Load Balancers
   - Select "WebApp-ALB"
   - **Actions** → **Delete load balancer**
   - Type "confirm" to delete
   - Wait for deletion (1-2 minutes)

4. **Delete Target Group:**
   - Go to Target Groups
   - Select "WebApp-TG"
   - **Actions** → **Delete**
   - Confirm deletion

5. **Delete Launch Template:**
   - Go to Launch Templates
   - Select "WebServer-Template"
   - **Actions** → **Delete template**
   - Confirm deletion

6. **Delete Security Groups:**
   - Go to Security Groups
   - Select "ALB-SG" → **Actions** → **Delete security groups**
   - Select "ALB-WebServer-SG" → **Actions** → **Delete security groups**
   - **Note:** May need to wait if dependencies exist

7. **Verify cleanup:**
   - No running or pending EC2 instances from this lab
   - No load balancers in list
   - No Auto Scaling groups in list
   - Target group deleted

**Cost Warning:**
- Load Balancers: $0.0225/hour (~$16/month) - NOT free tier eligible
- EC2 instances: Free if within 750 hours/month on t2.micro
- **Leaving ALB running overnight = ~$0.54 wasted**

**Verification Commands (optional):**
```bash
aws elbv2 describe-load-balancers --region us-east-1
aws autoscaling describe-auto-scaling-groups --region us-east-1
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --region us-east-1
```

### Post-Lab Knowledge Check

**Question 1:** What's the difference between desired, minimum, and maximum capacity?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
- **Desired capacity:** Current target number of instances Auto Scaling maintains
- **Minimum capacity:** Lowest number of instances (never goes below this)
- **Maximum capacity:** Highest number of instances (never exceeds this)

Example: Min=1, Desired=2, Max=4
- Normal operation: 2 instances running
- During scale-out: Can add up to 2 more (total 4)
- During scale-in: Can remove 1 (minimum is 1)
- Desired capacity changes dynamically, min/max are boundaries

</details>

**Question 2:** Why use target tracking instead of step scaling?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
- **Target tracking:** Simpler to configure, automatically calculates scaling adjustments to maintain target (like a thermostat). Best for most use cases.
- **Step scaling:** More control, define specific scaling amounts for different threshold ranges. Use for complex scaling patterns.
- **Exam tip:** Target tracking is recommended by AWS for most scenarios and is the default option.

</details>

**Question 3:** What happens if an instance fails health checks?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
1. Target group marks instance "unhealthy" after 2 failed checks (configurable)
2. Load balancer stops sending traffic to that instance
3. Auto Scaling detects unhealthy instance
4. After grace period, Auto Scaling terminates unhealthy instance
5. Auto Scaling launches replacement instance to maintain desired capacity
6. New instance goes through health checks before receiving traffic

This provides self-healing infrastructure!

</details>

**Question 4:** Can Auto Scaling work without a load balancer?

<details>
<summary>Click to reveal answer</summary>

**Answer:** Yes! Auto Scaling works independently of load balancers.

**Without ELB:**
- Instances still launch/terminate based on policies
- Use cases: Batch processing, worker nodes, background jobs
- Health checks based on EC2 status only

**With ELB:**
- Better for web apps serving user traffic
- Health checks from both ELB and EC2
- Traffic distributed automatically
- More resilient architecture

**Exam tip:** Know that Auto Scaling and ELB are separate services that work well together but aren't required together.

</details>

**Question 5:** What's the purpose of the warmup period?

<details>
<summary>Click to reveal answer</summary>

**Answer:** Warmup period (300 seconds recommended) prevents new instances from being evaluated for scaling before they're ready.

**Without warmup:**
- New instance launched (CPU low while starting)
- Average CPU of group drops
- Might trigger premature scale-in
- Instability and thrashing

**With warmup:**
- New instance launches
- Metrics ignored for 300 seconds
- Instance has time to initialize and receive traffic
- Stable scaling behavior

</details>

### Key Takeaways

- **Auto Scaling provides elasticity** - capacity matches demand automatically
- **ELB distributes traffic** - no single point of failure
- **Target tracking is simplest** - set target, AWS handles the rest
- **Health checks are critical** - determines which instances receive traffic
- **Multi-AZ deployment** - provides high availability
- **Launch templates** - define instance configuration once, reuse many times
- **Cooldown periods prevent thrashing** - avoids rapid scale in/out cycles
- **Cost optimization** - only pay for what you need, when you need it

---

## Lab 12: DynamoDB Hands-On

**Duration:** 35 minutes
**Cost:** Free (25 GB storage always free)
**Difficulty:** Intermediate

### Learning Objectives

By the end of this lab, you will be able to:

1. Create a DynamoDB table with partition and sort keys
2. Understand DynamoDB data types and attributes
3. Add, query, and scan items using the AWS Console
4. Create and use Global Secondary Indexes (GSI)
5. Configure DynamoDB auto-scaling
6. Understand read/write capacity modes
7. Export table data and enable point-in-time recovery

### Why This Lab Matters

**Real-World Scenario:** A mobile gaming company uses DynamoDB to store player profiles, game scores, and real-time leaderboards. DynamoDB handles millions of requests per day with single-digit millisecond latency, automatically scaling without server management.

**Exam Relevance:** DynamoDB is a key AWS service. Know:
- NoSQL vs. relational databases
- Primary keys (partition key, sort key)
- Indexes (LSI, GSI)
- Capacity modes (on-demand vs. provisioned)
- DynamoDB Accelerator (DAX) for caching

### Prerequisites

- Basic understanding of databases (SQL or NoSQL)
- Knowledge of data structures (key-value pairs)
- Completed Lab 1 (billing alerts recommended)

### Step-by-Step Instructions

#### Part 1: Create DynamoDB Table

1. Navigate to **DynamoDB** service
2. Click **"Create table"**

3. **Table details:**
   - **Table name:** "GameScores"
   - **Partition key:** "UserId" (String)
   - **Sort key:** "GameTitle" (String)
   - **Why these keys:**
     - Partition key distributes data across partitions
     - Sort key orders items within each partition
     - Together form unique composite key
     - Example: User123 + Minecraft, User123 + Fortnite

4. **Table settings:**
   - Select **"Customize settings"** (not Default settings)

5. **Table class:**
   - Select **"DynamoDB Standard"**
   - (Standard-IA is for infrequently accessed data)

6. **Read/write capacity settings:**
   - **Capacity mode:** On-demand
   - **Why on-demand:** Automatically scales, no capacity planning, pay per request
   - **Alternative:** Provisioned mode (Free Tier: 25 WCU + 25 RCU)
   - **Best for lab:** On-demand (simpler, less chance of throttling)

7. **Secondary indexes:**
   - Skip for now (add later in lab)

8. **Encryption at rest:**
   - **Encryption type:** Owned by Amazon DynamoDB
   - (Default, no additional cost)

9. **Tags (optional):**
   - Key: Project, Value: DynamoDB-Lab

10. Click **"Create table"**
11. Wait 10-20 seconds for status: "Active"

**Validation:**
- Table appears in Tables list
- Status shows "Active"
- Table details show partition and sort keys

#### Part 2: Add Items to Table

1. Click on **"GameScores"** table name
2. Click **"Explore table items"** button
3. Click **"Create item"**

**Item 1:**
4. **Add attributes:**
   - UserId (String): "User001"
   - GameTitle (String): "Minecraft"
5. Click **"Add new attribute"** → **Number**
   - Attribute name: "Score"
   - Value: 1250
6. Click **"Add new attribute"** → **Number**
   - Attribute name: "Level"
   - Value: 15
7. Click **"Add new attribute"** → **String**
   - Attribute name: "PlayerName"
   - Value: "Steve"
8. Click **"Add new attribute"** → **Number**
   - Attribute name: "Timestamp"
   - Value: 1699564800 (Unix timestamp)
9. Click **"Create item"**

**Item 2:**
10. Click **"Create item"** again
11. Add attributes:
    - UserId: "User001"
    - GameTitle: "Fortnite"
    - Score: 2400
    - Level: 22
    - PlayerName: "Steve"
    - Timestamp: 1699651200

**Item 3:**
12. Create third item:
    - UserId: "User002"
    - GameTitle: "Minecraft"
    - Score: 980
    - Level: 12
    - PlayerName: "Alex"
    - Timestamp: 1699737600

**Item 4:**
13. Create fourth item:
    - UserId: "User002"
    - GameTitle: "Fortnite"
    - Score: 3100
    - Level: 28
    - PlayerName: "Alex"
    - Timestamp: 1699824000

**Item 5:**
14. Create fifth item:
    - UserId: "User003"
    - GameTitle: "Minecraft"
    - Score: 1800
    - Level: 18
    - PlayerName: "Herobrine"
    - Timestamp: 1699910400

**What You Should See:**
- Items appear in table immediately
- Each item has UserId + GameTitle (keys) plus additional attributes
- Items can have different attributes (schema-less)
- Scan shows all items

#### Part 3: Query Items

> **Query vs. Scan:** Query is efficient (uses keys), Scan reads entire table (slow, expensive).

1. Click **"Query"** (default view after adding items)
2. **Query items where:**
   - Partition key: UserId
   - **Enter:** "User001"
3. Click **"Run"**

**Results:**
- Shows 2 items: Minecraft and Fortnite for User001
- Sorted by GameTitle (sort key)
- Fast query using primary key

4. **Add sort key condition:**
   - Partition key: "User001"
   - **Sort key condition:** GameTitle = "Minecraft"
5. Click **"Run"**

**Results:**
- Shows only 1 item: User001's Minecraft score
- Even more specific query

6. **Try another query:**
   - Partition key: "User002"
   - Leave sort key empty
7. Click **"Run"**

**Results:**
- Shows both games for User002

**Real-World Use:** Query player's specific game score or all games for a player

#### Part 4: Scan Items

1. Click **"Scan"** tab (next to Query)
2. Click **"Run"**

**Results:**
- Shows all 5 items from table
- No filtering applied
- **Warning:** Scans are expensive for large tables (read all data)

3. **Add scan filter:**
   - Click **"Filters"**
   - **Add filter:**
     - Attribute name: Score
     - Condition: Greater than or equal to
     - Value: 2000
   - Click **"Run"**

**Results:**
- Shows only 2 items: User001 Fortnite (2400), User002 Fortnite (3100)
- Filtered after scanning entire table
- **Note:** Still scans all items then filters (not as efficient as query)

**Best Practice:** Use queries with keys whenever possible; scans for analytics only

#### Part 5: Create Global Secondary Index (GSI)

**Problem:** What if we want to find all players with Score > 2000 efficiently?
**Solution:** Create GSI with Score as partition key

1. Go to **"Indexes"** tab
2. Click **"Create index"**

3. **Index details:**
   - **Partition key:** Score (Number)
   - **Sort key:** Timestamp (Number) (optional, but useful for ordering)
   - **Index name:** "ScoreIndex"
   - **Attribute projections:** All
     - Projects all table attributes into index
     - Alternative: Keys only (smaller, cheaper) or Include (specify attributes)

4. Click **"Create index"**
5. Wait 20-30 seconds for status: "Active"

**Validation:**
- Index shows in Indexes tab
- Status: Active
- Can now query by Score efficiently

#### Part 6: Query Using GSI

1. Go back to **"Explore table items"**
2. Click **"Query"** tab
3. **Index:** Select "ScoreIndex" from dropdown
4. **Query items:**
   - Partition key (Score): 1250
5. Click **"Run"**

**Results:**
- Shows User001's Minecraft score
- Queried by Score instead of UserId

6. **Try range query on GSI:**
   - Unfortunately, DynamoDB Query requires exact partition key value
   - For range queries on Score, must use Scan with filter (limitation of DynamoDB)
   - **Alternative approach:** Create items with score ranges as partition keys (advanced)

**Real-World Use:**
- GSI for querying by non-key attributes
- Common pattern: UserId as partition key, GSI on Email for login lookups
- Up to 20 GSIs per table

#### Part 7: Update Item

1. In "Explore table items" view, select an item (click radio button)
2. Click **"Actions"** → **"Edit item"**
3. Change Score from 1250 to 1300
4. Click **"Add new attribute"** → **String**
   - Attribute name: "Achievement"
   - Value: "Master Builder"
5. Click **"Save changes"**

**What You Should See:**
- Item updated immediately
- New attribute added
- Other items not affected (schema-less flexibility)

#### Part 8: Delete Item

1. Select an item (checkbox)
2. Click **"Actions"** → **"Delete items"**
3. Confirm deletion
4. Item removed immediately

**Restore item (practice adding):**
- Click "Create item" and re-add the deleted item

#### Part 9: Configure Table Settings

1. Go to **"Additional settings"** tab

**Point-in-time recovery (PITR):**
2. Scroll to **"Point-in-time recovery"** section
3. Click **"Edit"**
4. Select **"Turn on"**
5. Click **"Save changes"**
   - **What it does:** Continuous backups, restore to any point in last 35 days
   - **Cost:** Additional charge based on table size
   - **Exam tip:** PITR protects against accidental deletes/updates

**Time to Live (TTL):**
6. Scroll to **"Time to Live (TTL)"** section
7. Click **"Edit"**
8. **Turn on** TTL
9. **TTL attribute:** "ExpiresAt"
10. Click **"Save changes"**
    - **What it does:** Automatically deletes items after expiration time
    - **Use case:** Session data, temporary records
    - **Cost:** Free (deletion doesn't consume write capacity)

**Note:** We didn't add ExpiresAt to our items, so TTL won't affect them

#### Part 10: Export Table Data

1. Go to **"Exports and streams"** tab
2. Click **"Export to S3"**
3. **Destination S3 bucket:**
   - Click **"Browse S3"**
   - Select existing bucket or create new one: "dynamodb-exports-[yourname]"
4. **Export format:** DynamoDB JSON
5. Click **"Export"**
6. Export status: "In progress" → "Completed" (2-3 minutes)

7. **View exported data:**
   - Go to S3 → Your export bucket
   - Navigate through folders to find data file
   - Download and view JSON export

**Use cases:**
- Data analysis with Athena
- Backup and archival
- Data migration
- Compliance requirements

### Expected Outcomes

- DynamoDB table created with composite primary key
- Multiple items added with various attributes
- Successfully queried items using partition key
- Scanned table with filters applied
- Created Global Secondary Index for alternative queries
- Updated and deleted items
- Configured Point-in-time Recovery and TTL
- Exported table data to S3

### Verification Checklist

- [ ] Table "GameScores" shows "Active" status
- [ ] At least 5 items in table
- [ ] Query by UserId returns correct items
- [ ] Scan with filter shows expected results
- [ ] Global Secondary Index "ScoreIndex" is active
- [ ] Can query using GSI
- [ ] Point-in-time recovery enabled
- [ ] Successfully exported to S3

### Real-World Tips

**Partition Key Design:**
- High cardinality (many unique values) for even distribution
- Avoid "hot partitions" (one key getting most traffic)
- Bad example: Date as partition key (all today's data on one partition)
- Good example: CustomerId (distributed across customers)

**When to Use DynamoDB:**
- **Yes:** High-scale applications, gaming, IoT, mobile backends, real-time bidding
- **Yes:** Serverless applications (pairs well with Lambda)
- **Yes:** Key-value access patterns
- **No:** Complex joins (use RDS instead)
- **No:** Ad-hoc queries (use Athena + S3 or RDS)
- **No:** ACID transactions across tables (use RDS)

**Capacity Mode Selection:**
- **On-demand:** Unpredictable workloads, new applications, pay-per-request
- **Provisioned:** Predictable traffic, steady state, cost optimization (up to 60% savings)
- **Can switch:** Once per 24 hours between modes

**Cost Optimization:**
- Use on-demand for development, provisioned for production
- Enable auto-scaling for provisioned mode
- Use S3 + Athena for analytics instead of scans
- Archive old data to S3 using TTL + streams
- Standard-IA class for infrequently accessed data

### Troubleshooting

**Problem:** "Validation Exception" when creating item

**Solution:**
- Verify partition key and sort key values provided
- Check attribute names don't have typos
- Ensure data types match (String vs. Number)
- Partition and sort keys are required fields

**Problem:** Query returns no results

**Solution:**
- Verify exact partition key value (case-sensitive)
- Check you're using correct index (table vs. GSI)
- Confirm items exist with that partition key
- Try scan to see all items first

**Problem:** Cannot create GSI - limit exceeded

**Solution:**
- Free Tier allows up to 20 GSIs per table
- Delete unused indexes before creating new ones
- Consider if you really need GSI (scan might be acceptable)

**Problem:** Export to S3 fails

**Solution:**
- Verify S3 bucket exists and in same region
- Check DynamoDB has permissions to write to bucket
- Ensure bucket name is globally unique
- Review export status details for specific error

**Problem:** High read/write costs

**Solution:**
- Check for scan operations (use queries instead)
- Review on-demand vs. provisioned pricing
- Implement caching layer (DAX or ElastiCache)
- Use eventually consistent reads (50% cheaper)

### Cleanup

> **Good News:** DynamoDB charges only for storage and requests. With 25 GB always free, small tables are essentially free.

**Option 1: Keep Table (Recommended for Learning)**
- Minimal cost with 5 items (<1 KB storage)
- Good for practicing queries
- Can experiment further

**Option 2: Delete Table**

1. Go to **DynamoDB** → **Tables**
2. Select **"GameScores"** table
3. Click **"Delete"**
4. **Delete all CloudWatch alarms for this table:** Check box
5. **Create a backup before deleting:** Uncheck (for lab purposes)
6. Type **"delete"** to confirm
7. Click **"Delete table"**

8. **Delete S3 export bucket (if created):**
   - Go to S3
   - Select export bucket
   - Click **"Empty"** → Type "permanently delete" → Empty
   - Click **"Delete"** → Type bucket name → Delete

**Verification:**
- Table no longer in DynamoDB tables list
- Export bucket deleted from S3
- No ongoing charges

**Cost Note:**
- On-demand mode: $0 with no traffic
- 5 items < 1 KB: ~$0.00025/month storage
- Effectively free to keep for practice

### Post-Lab Knowledge Check

**Question 1:** What's the difference between partition key and sort key?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
- **Partition key (Hash key):** Required, determines which partition stores the item, must be unique if used alone
- **Sort key (Range key):** Optional, orders items within a partition, enables range queries
- **Together:** Form composite primary key (partition + sort), partition key groups items, sort key orders within group

Example: UserId (partition) + Timestamp (sort) allows querying all actions for a user, ordered by time

</details>

**Question 2:** When should you use a Global Secondary Index?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
Use GSI when you need to query by attributes other than the primary key.

**Example scenarios:**
- Table has UserId as partition key, need to query by Email → Create GSI with Email as partition key
- Table has OrderId as partition key, need to find all orders for a CustomerId → GSI on CustomerId
- Need different sort order → GSI with different sort key

**Limitations:**
- Eventually consistent (slight delay)
- Consumes additional write capacity
- Costs extra storage (projects attributes)
- Cannot be changed after creation (must delete and recreate)

</details>

**Question 3:** What's the difference between Query and Scan?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
- **Query:** Efficient, uses partition key (and optionally sort key), returns only matching items, low latency, predictable cost
- **Scan:** Inefficient, reads entire table, filters after reading, high latency for large tables, expensive

**Example:**
- Query for "UserId=User001": Reads only User001's items
- Scan with filter "UserId=User001": Reads ALL items, then filters

**Exam tip:** Always prefer Query over Scan. Use Scan only for analytics or one-time operations.

</details>

**Question 4:** What is DynamoDB Accelerator (DAX)?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
DAX is an in-memory cache for DynamoDB providing microsecond latency.

**Features:**
- Fully managed, highly available cache
- Reduces read latency from milliseconds to microseconds
- No application code changes (drop-in compatible)
- Supports eventually consistent and strongly consistent reads

**Use cases:**
- Read-heavy workloads
- Gaming leaderboards
- Real-time bidding
- Applications requiring <1ms response

**Note:** Not included in Free Tier, costs apply

</details>

**Question 5:** How does DynamoDB pricing work?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
**On-Demand Mode:**
- Pay per request: $1.25 per million write requests, $0.25 per million read requests
- Storage: $0.25 per GB/month
- Best for unpredictable workloads

**Provisioned Mode:**
- Reserve capacity: $0.00065 per WCU-hour, $0.00013 per RCU-hour
- Auto-scaling available
- Best for predictable, steady workloads
- Free Tier: 25 WCU + 25 RCU + 25 GB storage

**Additional costs:**
- Backups, restores, global tables, streams
- Data transfer out

**Exam tip:** Know the difference between capacity modes and when to use each

</details>

**Question 6:** Can DynamoDB handle relational data like SQL databases?

<details>
<summary>Click to reveal answer</summary>

**Answer:**
DynamoDB is NoSQL - not designed for relational patterns like JOINs.

**What DynamoDB CAN'T do well:**
- Multi-table joins
- Complex aggregations
- Ad-hoc queries
- Referential integrity constraints

**What DynamoDB DOES well:**
- Key-value lookups
- Single-table design patterns
- High-scale applications
- Low-latency requirements

**Best Practice:**
- Denormalize data (duplicate information across items)
- Single-table design (advanced pattern)
- Use RDS/Aurora for complex relational needs

**Exam tip:** Know when to use DynamoDB vs. RDS

</details>

### Key Takeaways

- **DynamoDB is NoSQL** - schema-less, scalable, managed service
- **Primary key is critical** - determines data distribution and access patterns
- **Query > Scan** - always design for query access patterns
- **GSIs enable flexibility** - query by non-key attributes
- **25 GB storage always free** - great for learning and small apps
- **On-demand mode** - simplest for beginners, no capacity planning
- **Point-in-time recovery** - protection against mistakes
- **Use with Lambda** - perfect for serverless architectures
- **Not a replacement for RDS** - choose right database for use case

### Additional Resources

- [DynamoDB Developer Guide](https://docs.aws.amazon.com/dynamodb/)
- [Best Practices for DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html)
- [DynamoDB Data Modeling](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/data-modeling.html)
- [AWS re:Invent DynamoDB Sessions](https://www.youtube.com/results?search_query=aws+reinvent+dynamodb)

---

---

## Troubleshooting FAQ

This section addresses common issues encountered across all labs.

### General AWS Console Issues

**Q: I can't find a service in the AWS Console**

A:
- Use the search bar at the top (type service name)
- Check if you're in the correct region (some services are region-specific)
- Verify your IAM user has permissions to access that service
- Some services have different names (e.g., "Billing and Cost Management" vs. "Billing")

**Q: AWS Console is very slow or timing out**

A:
- Clear browser cache and cookies
- Try incognito/private browsing mode
- Switch to a different browser (Chrome, Firefox, Edge)
- Check your internet connection (minimum 5 Mbps recommended)
- Try a different region (some regions may have connectivity issues)
- Check AWS Service Health Dashboard: https://status.aws.amazon.com

**Q: I'm getting "You are not authorized to perform this operation" errors**

A:
- Check IAM user has appropriate permissions (policies attached)
- If using IAM user, ensure root account enabled IAM billing access (for billing operations)
- Wait 5-10 minutes after creating IAM user (policy propagation delay)
- Try logging out and back in
- Verify you're in correct account (check account ID)

**Q: Resources I created don't appear in the console**

A:
- **Most common:** Wrong region selected (check region dropdown in top right)
- Wait 30-60 seconds and refresh (eventual consistency)
- Check filters applied in console (clear all filters)
- Verify resource actually created (check for error messages)
- Check CloudTrail for creation events

### Billing and Cost Issues

**Q: I'm being charged even though I'm using Free Tier**

A:
- Check Free Tier Dashboard for usage vs. limits
- Verify instance types are Free Tier eligible (t2.micro, t3.micro)
- Check for resources in multiple regions (Free Tier is per-account, not per-region)
- Look for non-Free Tier services (NAT Gateway, Load Balancers)
- Data transfer charges (out to internet)
- EBS storage beyond 30 GB
- RDS Multi-AZ (not Free Tier eligible)

**Q: My billing alarm isn't working**

A:
- Verify billing alarm created in us-east-1 region only
- Check SNS subscription confirmed (look for confirmation email)
- Wait 24 hours for initial data (billing metrics take time to populate)
- Ensure charges actually exceed threshold
- Verify alarm state is "OK" not "Insufficient data"

**Q: I can't access billing dashboard**

A:
- Must be logged in as root user OR
- IAM user with billing permissions AND root enabled IAM billing access
- Go to Account settings → Enable "Activate IAM Access" to billing

**Q: Unexpected charges after Free Tier ended**

A:
- Free Tier expires 12 months after account creation (check start date)
- Some services are "always free" (Lambda 1M requests, DynamoDB 25GB)
- Set up billing alerts for post-Free Tier monitoring
- Review bill in detail to identify costly services
- Consider shutting down non-essential resources

### EC2 Issues

**Q: Cannot connect to EC2 instance via SSH**

A:
1. **Connection timeout:**
   - Security group allows SSH (port 22) from your IP
   - Instance in public subnet with public IP
   - Route table has internet gateway route (0.0.0.0/0 → igw-xxx)
   - Network ACL allows SSH traffic
   - Instance is in "running" state

2. **Permission denied (publickey):**
   - Using correct .pem key file
   - Key file has proper permissions: `chmod 400 key.pem`
   - Using correct username (ec2-user for Amazon Linux, ubuntu for Ubuntu)
   - Key pair matches instance

3. **Host key verification failed:**
   - Type "yes" to accept fingerprint
   - Or use: `ssh -o StrictHostKeyChecking=no -i key.pem ec2-user@IP`

**Q: Instance status checks failing**

A:
- **1/2 checks passed:** System reachability failed (AWS hardware issue - stop/start instance)
- **0/2 checks passed:** Both system and instance checks failing (check logs, bad user data script)
- Wait 2-3 minutes after launch (checks take time)
- View System Log and Instance Screenshot from Actions menu

**Q: User data script didn't run**

A:
- SSH to instance: `cat /var/log/cloud-init-output.log`
- Look for errors in script execution
- Check syntax (bash scripts need #!/bin/bash)
- Ensure script has proper permissions
- User data runs only on first boot (unless configured otherwise)

**Q: Can't access web server on EC2**

A:
- Security group allows HTTP (port 80) from 0.0.0.0/0
- Web server actually running: `sudo systemctl status httpd` or `nginx`
- Check from inside: `curl localhost` (should work)
- Using HTTP not HTTPS (http:// not https://)
- Check firewall on instance: `sudo iptables -L`

**Q: Instance type not available / capacity error**

A:
- Try different Availability Zone within same region
- Try slightly different instance type (t2.micro vs t3.micro)
- Wait 30 minutes and retry (capacity fluctuates)
- Consider different region
- For persistent issues, contact AWS Support

### S3 Issues

**Q: 403 Forbidden error when accessing S3 website**

A:
- Bucket policy allows public read (`s3:GetObject` for Principal: *)
- "Block all public access" is OFF
- Static website hosting enabled
- Objects actually uploaded to bucket
- Bucket policy ARN includes `/*` at end (`arn:aws:s3:::bucket-name/*`)
- Correct bucket website endpoint URL (not regular S3 URL)

**Q: 404 Not Found error on S3 website**

A:
- index.html file exists in bucket root (case-sensitive)
- File name exactly "index.html" (not Index.html or index.HTML)
- Static website hosting enabled
- Check if using correct endpoint (website endpoint, not REST endpoint)
- Clear browser cache

**Q: Cannot create bucket - name already exists**

A:
- S3 bucket names are globally unique across all AWS accounts
- Try different name with random numbers: `my-bucket-12345678`
- Bucket names must be DNS-compliant (lowercase, no underscores)
- Someone else may have that name (even if deleted <24 hours ago)

**Q: Cannot delete bucket - "Bucket not empty"**

A:
1. Go to bucket
2. Click "Empty" button
3. Type "permanently delete"
4. Wait for emptying to complete
5. Then click "Delete bucket"
- Alternative: Enable versioning → Delete all versions → Then delete bucket
- Check for incomplete multipart uploads

**Q: S3 uploads are very slow**

A:
- Check internet connection speed
- Try uploading smaller files first
- Use multipart upload for files >100 MB
- Consider using AWS CLI or SDKs (faster than console)
- Check if browser extensions interfering

### VPC and Networking Issues

**Q: EC2 instance has no internet access**

A:
- Instance in public subnet (check subnet settings)
- Instance has public IP or Elastic IP
- Subnet's route table has route to Internet Gateway (0.0.0.0/0 → igw-xxx)
- Security group allows outbound traffic (default allows all)
- Network ACL allows outbound traffic (default allows all)
- DNS resolution enabled for VPC

**Q: Cannot SSH between EC2 instances in same VPC**

A:
- Security groups allow traffic between instances
- Use private IPs (not public IPs) for same-VPC communication
- Both instances in subnets with proper routing
- Network ACLs allow traffic (if custom NACLs)

**Q: VPC creation fails**

A:
- Check VPC limit not exceeded (5 per region by default)
- CIDR block doesn't overlap with existing VPCs (if VPC peering planned)
- CIDR block valid (/16 to /28 for VPC)
- Try different region if persistent issues

**Q: Security group changes don't take effect**

A:
- Wait 30-60 seconds (eventual consistency)
- Refresh console page
- Check correct security group attached to instance
- Verify rule syntax (port ranges, protocols, sources)
- Security groups are stateful (don't need outbound rules for responses)

### RDS Issues

**Q: Cannot connect to RDS from EC2**

A:
- RDS and EC2 in same VPC
- RDS security group allows MySQL/PostgreSQL port from EC2 security group
- Using RDS endpoint hostname (not IP address)
- Correct port (3306 for MySQL, 5432 for PostgreSQL)
- RDS status is "Available"
- Credentials correct (case-sensitive)
- Not trying to connect from public internet (RDS public access = No)

**Q: RDS creation is slow**

A:
- Normal: RDS takes 5-15 minutes to create
- Multi-AZ takes longer (15-20 minutes)
- Watch status: Creating → Backing up → Available
- If stuck for >30 minutes, check CloudTrail for errors

**Q: RDS costs more than expected**

A:
- Check instance class (db.t3.micro is Free Tier)
- Multi-AZ doubles costs (not Free Tier eligible)
- Backup storage over 20 GB
- Provisioned IOPS costs extra
- Enabled Enhanced Monitoring ($)
- Verify "Free Tier" template used during creation

### IAM Issues

**Q: IAM user can't sign in**

A:
- Using IAM user sign-in URL (not root sign-in page)
- Sign-in URL format: `https://account-id.signin.aws.amazon.com/console`
- Or: `https://account-alias.signin.aws.amazon.com/console`
- Username and password correct (case-sensitive)
- User has console access enabled (not just programmatic)
- Account not locked after multiple failed attempts (wait 15 minutes)

**Q: IAM user has no permissions despite policy attached**

A:
- Wait 5-10 minutes for policy propagation
- Check policy attached to user OR group user belongs to
- Policy JSON syntax correct (use policy validator)
- No explicit Deny statements (Deny overrides Allow)
- Check if SCPs (Service Control Policies) limiting access (AWS Organizations)

**Q: Cannot enable MFA**

A:
- Phone time synchronized with internet time
- Scan QR code successfully with authenticator app
- Enter two consecutive codes (not same code twice)
- Codes entered quickly (expire every 30 seconds)
- Try different authenticator app if persistent issues

### Lambda Issues

**Q: Lambda function fails with timeout**

A:
- Increase timeout setting (default 3 seconds, max 15 minutes)
- Check function actually completes within timeout
- Look for infinite loops or blocking operations
- Review CloudWatch logs for execution time

**Q: Lambda function fails with "Permission denied"**

A:
- Lambda execution role has required permissions
- If accessing S3: Role needs `s3:GetObject`, `s3:PutObject`
- If accessing DynamoDB: Role needs appropriate DynamoDB permissions
- Check CloudWatch Logs for specific permission errors

**Q: API Gateway returns "Internal Server Error"**

A:
- Check Lambda function CloudWatch logs for actual error
- Function returning proper response format (statusCode, body, headers)
- Integration configured correctly (Lambda proxy integration recommended)
- API deployed (must redeploy after changes)

**Q: Lambda function can't connect to RDS/internet**

A:
- If Lambda in VPC: VPC needs NAT Gateway for internet access
- Security groups allow traffic between Lambda and RDS
- Lambda execution role has `ec2:CreateNetworkInterface` permission
- Timeout increased (VPC adds latency)

### CloudFormation Issues

**Q: Stack creation failed - rollback initiated**

A:
- Check "Events" tab for specific error message
- Common: Resource name conflict (already exists)
- Common: Insufficient permissions
- Common: Invalid parameter values
- Fix template and create new stack (or update if supported)

**Q: Stack stuck in CREATE_IN_PROGRESS**

A:
- Check Events for last completed action
- Some resources take time (RDS 10-15 min, NAT Gateway 3-5 min)
- If truly stuck >30 min, delete stack and recreate
- Check service limits (might be at capacity)

**Q: Can't delete stack - resource dependencies**

A:
- Empty S3 buckets before deleting stack
- Remove ENIs (Elastic Network Interfaces) attached to Lambda/RDS
- Delete dependencies manually then retry
- Check "Retain" resource policy (some resources protected)

### Auto Scaling and Load Balancer Issues

**Q: Auto Scaling not launching instances**

A:
- Check Auto Scaling group Activity tab for errors
- Verify launch template valid (AMI exists, instance type available)
- Not at maximum capacity limit
- Subnet has available IP addresses
- Service limits not exceeded (EC2 instance limit)

**Q: Load Balancer shows all targets unhealthy**

A:
- Health check path correct (must return 200 status)
- Security group allows health check traffic from load balancer
- Application actually running on instances
- Health check interval/threshold appropriate (increase grace period)
- Check target group health check settings

**Q: Load Balancer returns 503 Service Unavailable**

A:
- No healthy targets available
- All targets failing health checks
- Target group has no registered targets
- Instances in correct subnets
- Wait for instances to pass health checks (2 consecutive successes)

### DynamoDB Issues

**Q: Query returns no results**

A:
- Partition key value exact match (case-sensitive)
- Using correct index (base table vs GSI)
- Items actually exist with that key
- Try Scan to verify items in table

**Q: DynamoDB throttling errors (ProvisionedThroughputExceededException)**

A:
- Switch to On-Demand capacity mode
- Or increase provisioned capacity (WCU/RCU)
- Enable Auto Scaling for provisioned mode
- Implement exponential backoff in application
- Check for hot partition (one key getting all traffic)

**Q: Cannot create GSI - limit exceeded**

A:
- Maximum 20 GSIs per table
- Delete unused indexes
- Consider if Scan with filter acceptable

### Common Error Messages Decoded

**"InvalidParameterValue":**
- A parameter you provided has invalid value
- Check AWS documentation for valid values
- Common: Wrong availability zone, invalid CIDR block, bad instance type

**"UnauthorizedOperation":**
- IAM user/role lacks required permission
- Add necessary policy to user/role
- Check typos in action names

**"ResourceNotFoundException":**
- Resource you're trying to access doesn't exist
- Verify resource ID correct
- Check correct region
- Resource may have been deleted

**"LimitExceeded":**
- Hit AWS service limit (EC2 instances, VPCs, security groups)
- Request limit increase via Service Quotas console
- Or clean up unused resources

**"DependencyViolation":**
- Trying to delete resource with dependencies
- Example: Can't delete security group attached to running instance
- Remove dependencies first

### Getting Help

**When to ask for help:**
- Tried all troubleshooting steps
- Issue persists for >30 minutes
- Billing concern (unexpected charges)
- Service outage suspected

**Where to get help:**
1. **AWS Documentation:** Most comprehensive - https://docs.aws.amazon.com
2. **AWS re:Post:** Community forum - https://repost.aws
3. **AWS Support:** If you have paid support plan
4. **Stack Overflow:** Tag questions with [amazon-web-services]
5. **AWS Service Health Dashboard:** Check for outages - https://status.aws.amazon.com

**Information to provide when asking for help:**
- AWS service name
- Region
- Exact error message
- Steps taken so far
- Screenshots (redact sensitive info)
- Resource IDs
- Timeline (when did issue start)

**What NOT to share:**
- AWS Access Keys or Secret Keys
- Passwords
- Full ARNs with account IDs (can be partially redacted)
- Credit card information

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
