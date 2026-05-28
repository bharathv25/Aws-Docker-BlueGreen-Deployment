 Step 1 — Create VPC
1. Go to AWS Console → search VPC
2. Click Your VPCs → Create VPC
3. Fill in:
   - Name: `my-vpc`
   - IPv4 CIDR: `10.0.0.0/16`
4. Click Create VPC

Step 2 — Create Public Subnet
1. Click Subnets → Create Subnet
2. Fill in:
   - VPC: `my-vpc`
   - Name: `public-subnet`
   - Availability Zone: `ap-south-1a`
   - IPv4 CIDR: `10.0.1.0/24`
3. Click Create Subnet

Step 3 — Create Private Subnet
1. Click Create Subnet again
2. Fill in:
   - VPC: `my-vpc`
   - Name: `private-subnet`
   - Availability Zone: `ap-south-1a`
   - IPv4 CIDR: `10.0.2.0/24`
3. Click Create Subnet

 Step 4 — Create Internet Gateway
1. Click Internet Gateways → Create Internet Gateway
2. Name: `my-igw`
3. Click Create
4. Click Attach to VPC → select `my-vpc`
5. Verify status shows *Attached*

 Step 5 — Create NAT Gateway
1. Click NAT Gateways → Create NAT Gateway
2. Fill in:
   - Name: `my-nat`
   - Subnet: `public-subnet` ⚠️ must be public
   - Connectivity: Public
   - Availability mode: Zonal
   - Elastic IP: Automatic
3. Wait for status to show *Available*

 Step 6 — Create Public Route Table
1. Click Route Tables → Create Route Table
2. Fill in:
   - Name: `public-rt`
   - VPC: `my-vpc`
3. Click Routes → Edit Routes → Add Route:
   - Destination: `0.0.0.0/0`
   - Target: `my-igw`
4. Click Subnet Associations → Edit → select `public-subnet`

Step 7 — Create Private Route Table
1. Click Create Route Table
2. Fill in:
   - Name: `private-rt`
   - VPC: `my-vpc`
3. Click Routes → Edit Routes → Add Route:
   - Destination: `0.0.0.0/0`
   - Target: `my-nat`
4. Click Subnet Associations → Edit → select `private-subnet`

 Key Concepts
- Public Subnet → has direct route to Internet Gateway → EC2 gets public IP
- Private Subnet → no direct internet → uses NAT Gateway for outbound only
- NAT Gateway→ allows Private EC2 to download packages but blocks inbound internet traffic
- Route Tables → control where network traffic is directed

Screenshots
