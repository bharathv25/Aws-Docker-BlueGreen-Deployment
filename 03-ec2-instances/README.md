03 - EC2 Instances

Overview
Two EC2 instances are launched — one in the public subnet (internet facing) and one in the private subnet (no public IP).

Instances Created
| Instance    | Subnet        | Public IP | Security Group |
|-------------|---------------|-----------|----------------|
| public-ec2  | public-subnet | Yes       | public-sg      |
| private-ec2 | private-subnet| No        | private-sg     |

 Key Pair
- Name: `my-key`
- Type: RSA
- Format: .pem
- Same key used for both instances for SSH jump access

Step by Step

Step 1 — Create Key Pair
1. Go to EC2 → Key Pairs → Create Key Pair
2. Fill in:
   - Name: `my-key`
   - Key pair type: RSA
   - Private key format: .pem
3. Click Create Key Pair
4. .pem file auto downloads → save it safely
   ⚠️ Cannot be downloaded again!

Step 2 — Launch Public EC2
1. Go to EC2 → Instances → Launch Instance
2. Fill in:
   - Name: `public-ec2`
   - AMI: Ubuntu Server 24.04 LTS
   - Instance type: t3.micro
   - Key pair: `my-key`
3. Network Settings → Edit:
   - VPC: `my-vpc`
   - Subnet: `public-subnet`
   - Auto assign public IP: Enable
   - Security Group: `public-sg`
4. Click Launch Instance

Step 3 — Launch Private EC2
1. Go to EC2 → Instances → Launch Instance
2. Fill in:
   - Name: `private-ec2`
   - AMI: Ubuntu Server 24.04 LTS
   - Instance type: t3.micro
   - Key pair: `my-key`
3. Network Settings → Edit:
   - VPC: `my-vpc`
   - Subnet: `private-subnet`
   - Auto assign public IP: Disable ⚠️
   - Security Group: `private-sg`
4. Click Launch Instance

Step 4 — SSH into Public EC2
Using EC2 Instance Connect:
1. Select public-ec2
2. Click Connect → EC2 Instance Connect → Connect

Step 5 — SSH into Private EC2 via Jump
Private EC2 has no public IP so we jump through Public EC2:

1. Copy key to Public EC2:

nano ~/.ssh/my-key.pem

Paste contents of my-key.pem

chmod 400 ~/.ssh/my-key.pem

SSH from Public EC2 to Private EC2:

ssh -i ~/.ssh/my-key.pem ubuntu@<private-ec2-private-ip>

Key Concepts

Public EC2 → internet facing, runs BookStack + Nginx + MySQL primary

Private EC2 → no public IP, runs MySQL replica only

Bastion Host → Public EC2 acts as jump server to reach Private EC2  

Same key pair → makes SSH jump easier

Screenshots
