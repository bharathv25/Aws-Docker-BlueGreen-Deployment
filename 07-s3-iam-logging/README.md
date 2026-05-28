07 - S3 + IAM Logging

Overview
MySQL general logs from the Primary database are automatically 
uploaded to an Amazon S3 bucket using an IAM Role attached 
to the Public EC2 instance. No access keys needed — IAM Role 
handles authentication securely.

Architecture
Public EC2
↓ (IAM Role - no keys needed)
MySQL Container
↓ general.log
S3 Bucket (mysql-logs)

Resources Created
- S3 Bucket → stores MySQL log files
- IAM Role → attached to Public EC2
- IAM Policy → AmazonS3FullAccess
- Shell Script → automates log upload

Why IAM Role instead of Access Keys?
- More secure — no credentials stored on server
- Auto rotates — no manual key management
- Best practice for EC2 to S3 communication

Step by Step

Step 1 — Create S3 Bucket
1. Go to AWS Console → S3
2. Click Create Bucket
3. Fill in:
   - Bucket name: unique name (e.g. `mysql-logs-yourname`)
   - Region: `ap-south-1`
   - Everything else → default
4. Click Create Bucket

Step 2 — Create IAM Role
1. Go to IAM → Roles → Create Role
2. Trusted entity: AWS Service → EC2
3. Permission: `AmazonS3FullAccess`
4. Role name: `ec2-s3-role`
5. Click Create Role

Step 3 — Attach IAM Role to Public EC2
1. Go to EC2 → Select public-ec2
2. Actions → Security → Modify IAM Role
3. Select `ec2-s3-role`
4. Click Update IAM Role

Step 4 — Enable MySQL General Logging

SET GLOBAL general_log = 'ON';

SET GLOBAL general_log_file = '/var/lib/mysql/general.log';

Step 5 — Install AWS CLI

sudo apt install awscli -y

Step 6 — Create Upload Script

nano upload-logs.sh

#!/bin/bash

# Copy log from container to host
sudo docker cp db:/var/lib/mysql/general.log /tmp/mysql-general.log

# Fix permissions
sudo chmod 644 /tmp/mysql-general.log

# Upload to S3
sudo aws s3 cp /tmp/mysql-general.log s3://your-bucket-name/logs/general-$(date +%Y%m%d-%H%M%S).log

echo "Log uploaded successfully!"

Step 7 — Run Script

chmod +x upload-logs.sh

./upload-logs.sh

Step 8 — Verify in S3

Go to S3 → your bucket → logs folder

Log file should appear with timestamp 

Key Concepts
IAM Role → gives EC2 permission to access S3 without keys

General Log → records every SQL query executed

Timestamped files → each upload creates new file

s3:PutObject → minimum permission needed to upload


