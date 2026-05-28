04 - Docker & BookStack

Overview
BookStack is deployed using Docker and Docker Compose on the 
Public EC2 instance. It is a open source wiki/documentation 
web application that uses MySQL as its database.

Why BookStack?
- Real world web application (not a toy app)
- MySQL dependent — fits our architecture perfectly
- Runs cleanly in Docker
- Professional looking for portfolio

Containers Running
| Container      | Image               | Port | Role            |
|----------------|---------------------|------|-----------------|
| bookstack-blue | solidnerd/bookstack | 8080 | Live version    |
| bookstack-green| solidnerd/bookstack | 9090 | Staging version |
| db             | mysql:8.0           | 3306 | Primary MySQL   |
| nginx          | nginx:latest        | 80   | Reverse Proxy   |

Step by Step

Step 1 — Install Docker on Public EC2

sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker ubuntu

Step 2 — Install Docker Compose

sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version

Step 3 — Generate APP Key
sudo docker run -it --rm --entrypoint /bin/bash \
lscr.io/linuxserver/bookstack:latest appkey

Copy the generated key — looks like:
base64:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

Step 4 — Create docker-compose.yml


Step 5 — Start Containers
sudo docker-compose up -d
sudo docker ps

Default Login
Email: admin@admin.com
Password: password
⚠️ Change password immediately after first login!

Key Concepts
Docker Compose → manages multiple containers together
depends_on → ensures MySQL starts before BookStack
volumes → persists MySQL data even if container restarts
restart: unless-stopped → auto restarts on crash
