5 - MySQL Replication

Overview
MySQL primary-replica replication is configured between 
Public EC2 (primary) and Private EC2 (replica). Any data 
written to BookStack automatically syncs to the replica.

Architecture
Public EC2 (Primary MySQL)
↓ Binary Log Replication (Port 3306)
Private EC2 (Replica MySQL)

Why Replication?
- High Availability → if primary fails, replica has all data
- Data backup → real time copy on separate server
- Read scaling → replica can handle read queries

Replication Setup

Primary (Public EC2) Configuration
MySQL started with these flags:

command: --server-id=1 --log-bin=binlog --binlog-format=ROW

command: --server-id=2

Step by Step
Step 1 — Create Replication User on Primary

CREATE USER 'replicator'@'%' IDENTIFIED BY 'your_password';
GRANT REPLICATION SLAVE ON *.* TO 'replicator'@'%';
FLUSH PRIVILEGES;
SHOW MASTER STATUS;

Note down the File and Position values from SHOW MASTER STATUS

Step 2 — Install Docker on Private EC2
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker

Step 3 — Run MySQL Replica on Private EC2
services:
  mysql-replica:
    image: mysql:8.0
    container_name: mysql-replica
    command: --server-id=2
    environment:
      - MYSQL_ROOT_PASSWORD=your_password
      - MYSQL_DATABASE=bookstack
      - MYSQL_USER=bookstack
      - MYSQL_PASSWORD=your_password
    ports:
      - 3306:3306
    restart: unless-stopped

Step 4 — Configure Replica
STOP SLAVE;
CHANGE MASTER TO
MASTER_HOST='<public-ec2-private-ip>',
MASTER_USER='replicator',
MASTER_PASSWORD='your_password',
MASTER_LOG_FILE='<file-from-step-1>',
MASTER_LOG_POS=<position-from-step-1>,
MASTER_SSL=0,
GET_MASTER_PUBLIC_KEY=1;
START SLAVE;
SHOW SLAVE STATUS\G

Step 5 — Verify Replication
Look for these two lines:
Slave_IO_Running: Yes
Slave_SQL_Running: Yes

Key Concepts
Binary Logging → MySQL records every change in binlog
IO Thread → replica connects to primary and reads binlog
SQL Thread → replica applies the binlog changes locally
server-id → must be unique for each MySQL instance

