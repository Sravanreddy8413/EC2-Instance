https://roadmap.sh/projects/ec2-instance

# AWS EC2 Instance

A beginner-level AWS and Linux DevOps project to launch an **Ubuntu EC2 instance**, connect to it using **SSH**, install **Nginx**, and deploy a simple static website.

---

## 🎯 Project Objective

The goal of this project is to gain hands-on experience with:

* AWS EC2
* Linux server administration
* SSH
* Security Groups
* Public IP addresses
* Nginx
* Static website deployment
* Basic cloud infrastructure

---

# 🏗️ Architecture

```text
                    Internet
                       |
                       |
                  Public IP
                       |
                       v
              +----------------+
              | AWS EC2        |
              | Ubuntu Server  |
              |                |
              | Nginx          |
              +-------+--------+
                      |
                      v
                 index.html
                      |
                      v
                Static Website
```

---

# 📁 Project Structure

```text
awsec2linuxdevops/
│
├── index.html
└── README.md
```

---

# ☁️ What is Amazon EC2?

**Amazon EC2 (Elastic Compute Cloud)** provides virtual servers in the AWS cloud.

An EC2 instance can be used to run:

* Websites
* APIs
* Databases
* Docker containers
* Jenkins
* Kubernetes components
* Monitoring tools
* CI/CD applications

Basic architecture:

```text
AWS
 |
 +-- Region
      |
      +-- Availability Zone
            |
            +-- VPC
                 |
                 +-- Subnet
                      |
                      +-- EC2 Instance
```

---

# 🧠 Important AWS EC2 Concepts

| Concept        | Description                               |
| -------------- | ----------------------------------------- |
| EC2            | Virtual server in AWS                     |
| AMI            | Template used to create an instance       |
| Instance Type  | Defines CPU, memory, and network capacity |
| VPC            | Virtual network in AWS                    |
| Subnet         | Network segment inside a VPC              |
| Security Group | Virtual firewall for an instance          |
| Key Pair       | Used for SSH authentication               |
| Public IP      | Allows internet connectivity              |
| Private IP     | Internal VPC address                      |
| EBS            | Persistent block storage for EC2          |

---

# 🛠️ Prerequisites

You need:

* AWS account
* AWS Management Console access
* SSH client
* Internet connection
* Basic Linux commands

For Windows, you can use:

* PowerShell
* Windows Terminal
* Git Bash
* WSL

---

# 1. Create an AWS Account

If you don't already have an AWS account, create one through the AWS website.

After signing in, open:

```text
AWS Management Console
```

Search for:

```text
EC2
```

Open the EC2 service.

> AWS pricing and Free Tier eligibility can change. Check the current AWS pricing/Free Tier terms before launching resources that may incur charges.

---

# 2. Choose an AWS Region

Select a region close to your users.

For example:

```text
Asia Pacific (Mumbai)
```

The exact region is your choice.

Check the selected region before launching the instance.

---

# 3. Launch an EC2 Instance

Go to:

```text
EC2 → Instances → Launch instance
```

Give the instance a name:

```text
devops-ec2-server
```

---

# 4. Select an AMI

Choose:

```text
Ubuntu Server
```

For a beginner project, use a current supported Ubuntu Server LTS AMI available in your selected AWS region.

The exact AMI ID changes by region and release.

---

# 5. Select Instance Type

The original project specifies:

```text
t2.micro
```

Select it if it is available and appropriate for your account.

AWS instance availability and Free Tier eligibility can vary by account, region, and current AWS program terms.

---

# 6. Create or Select a Key Pair

SSH requires a key pair.

Select:

```text
Create new key pair
```

Example:

```text
devops-ec2-key
```

Select:

```text
RSA
```

Download the private key:

```text
devops-ec2-key.pem
```

### Important

**Never upload the `.pem` private key to GitHub.**

Store it securely.

---

# 7. Configure Network

Use the default VPC and subnet for this beginner project.

Enable:

```text
Auto-assign Public IP
```

The instance needs a public IP so that you can connect from the internet.

---

# 8. Configure Security Group

Create a security group such as:

```text
devops-ec2-sg
```

Allow:

| Protocol | Port | Purpose               |
| -------- | ---: | --------------------- |
| SSH      |   22 | Remote administration |
| HTTP     |   80 | Website               |

For SSH, preferably restrict the source to **your IP address** rather than allowing the entire internet.

For a beginner lab, you may temporarily use a broader rule, but narrow it afterward.

Example:

```text
SSH
TCP
22
My IP

HTTP
TCP
80
0.0.0.0/0
```

---

# 🔐 Security Group Concept

A Security Group works like a virtual firewall around the EC2 instance.

```text
Internet
   |
   v
Security Group
   |
   +---- Port 22 ----> SSH
   |
   +---- Port 80 ----> Nginx
   |
   v
EC2 Instance
```

If port 22 is blocked:

```text
SSH ❌
```

If port 80 is blocked:

```text
Website ❌
```

---

# 9. Launch the Instance

Click:

```text
Launch instance
```

Wait until the instance state becomes:

```text
Running
```

Check:

```text
Instance State: Running
```

---

# 10. Find the Public IP

Open:

```text
EC2 → Instances
```

Select your instance.

Find:

```text
Public IPv4 address
```

Example:

```text
18.xxx.xxx.xxx
```

Do not hard-code this example; use the public IP assigned to your instance.

---

# 11. Connect Using SSH

On Linux/macOS/WSL/Git Bash:

First protect the private key:

```bash
chmod 400 devops-ec2-key.pem
```

Then connect:

```bash
ssh -i devops-ec2-key.pem ubuntu@<EC2_PUBLIC_IP>
```

Example:

```bash
ssh -i devops-ec2-key.pem ubuntu@18.xxx.xxx.xxx
```

---

# 12. Verify the Server

After connecting:

```bash
whoami
```

Expected:

```text
ubuntu
```

Check hostname:

```bash
hostname
```

Check OS:

```bash
cat /etc/os-release
```

Check IP:

```bash
ip addr
```

Check uptime:

```bash
uptime
```

---

# 13. Update Ubuntu

Run:

```bash
sudo apt update
```

Then:

```bash
sudo apt upgrade -y
```

This updates installed packages.

---

# 14. Install Nginx

Install Nginx:

```bash
sudo apt install nginx -y
```

Check status:

```bash
sudo systemctl status nginx
```

Expected:

```text
Active: active (running)
```

---

# 15. Enable Nginx at Boot

Run:

```bash
sudo systemctl enable nginx
```

This makes Nginx start automatically when the server boots.

---

# 16. Check Nginx

Check:

```bash
curl http://localhost
```

You should receive HTML from the Nginx default page.

Check listening ports:

```bash
sudo ss -lntp
```

You should see Nginx listening on port 80.

---

# 17. Create a Static Website

Nginx commonly serves static files from:

```text
/var/www/html
```

Create the HTML file:

```bash
sudo nano /var/www/html/index.html
```

Add:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DevOps EC2 Project</title>
</head>
<body>
    <h1>Hello from AWS EC2!</h1>
    <p>My first static website deployed on an Ubuntu EC2 server.</p>
</body>
</html>
```

Save the file.

---

# 18. Test the Website Locally

On the EC2 server:

```bash
curl http://localhost
```

You should see:

```text
Hello from AWS EC2!
```

You can also check:

```bash
curl -I http://localhost
```

Expected response:

```text
HTTP/1.1 200 OK
```

---

# 19. Open the Website from Your Browser

Open:

```text
http://<EC2_PUBLIC_IP>
```

Example:

```text
http://18.xxx.xxx.xxx
```

You should see:

```text
Hello from AWS EC2!
```

---

# 🌐 Website Request Flow

```text
Browser
   |
   | HTTP :80
   v
Internet
   |
   v
AWS Security Group
   |
   | Allow TCP 80
   v
EC2 Ubuntu
   |
   v
Nginx
   |
   v
/var/www/html/index.html
   |
   v
Website
```

---

# 20. Useful Nginx Commands

Start:

```bash
sudo systemctl start nginx
```

Stop:

```bash
sudo systemctl stop nginx
```

Restart:

```bash
sudo systemctl restart nginx
```

Reload:

```bash
sudo systemctl reload nginx
```

Status:

```bash
sudo systemctl status nginx
```

Enable at boot:

```bash
sudo systemctl enable nginx
```

---

# 21. Check Nginx Configuration

Before reloading Nginx:

```bash
sudo nginx -t
```

Expected:

```text
syntax is ok
test is successful
```

Then:

```bash
sudo systemctl reload nginx
```

---

# 22. Check Nginx Logs

Access log:

```bash
sudo tail -f /var/log/nginx/access.log
```

Error log:

```bash
sudo tail -f /var/log/nginx/error.log
```

These logs are useful for troubleshooting website requests and configuration problems.

---

# 23. Test HTTP Connectivity

From the EC2 server:

```bash
curl -I http://localhost
```

From your local machine:

```bash
curl -I http://<EC2_PUBLIC_IP>
```

Expected:

```text
HTTP/1.1 200 OK
```

---

# 24. Troubleshooting SSH

## Error: Permission denied

Check key permissions:

```bash
chmod 400 devops-ec2-key.pem
```

Use the correct Ubuntu username:

```bash
ssh -i devops-ec2-key.pem ubuntu@<EC2_PUBLIC_IP>
```

---

## Error: Connection timed out

Check:

1. EC2 instance is running
2. Public IP is correct
3. Security Group allows TCP 22
4. Your current public IP is allowed by the SSH rule
5. Network ACL/routing configuration is not blocking traffic

---

# 25. Troubleshooting Website

If the website does not open, check Nginx:

```bash
sudo systemctl status nginx
```

Check port 80:

```bash
sudo ss -lntp | grep :80
```

Check the Security Group:

```text
Inbound
TCP 80
Source: 0.0.0.0/0
```

Check Nginx configuration:

```bash
sudo nginx -t
```

Check logs:

```bash
sudo tail -f /var/log/nginx/error.log
```

---

# 26. Optional Ubuntu Firewall

Check UFW:

```bash
sudo ufw status
```

If UFW is enabled, allow SSH:

```bash
sudo ufw allow 22/tcp
```

Allow HTTP:

```bash
sudo ufw allow 80/tcp
```

Check:

```bash
sudo ufw status
```

Be careful when changing firewall rules over SSH. Always ensure SSH access remains allowed before enabling restrictive firewall policies.

---

# 27. Deploy Website Using SCP

Instead of creating the HTML directly on the server, you can create it locally.

Example local structure:

```text
website/
├── index.html
└── style.css
```

Copy the files:

```bash
scp -i devops-ec2-key.pem -r website/* ubuntu@<EC2_PUBLIC_IP>:/tmp/website/
```

Then SSH into the server:

```bash
ssh -i devops-ec2-key.pem ubuntu@<EC2_PUBLIC_IP>
```

Copy files to Nginx:

```bash
sudo cp -r /tmp/website/* /var/www/html/
```

Check:

```bash
curl http://localhost
```

---

# 28. Deploy Website Using rsync

A more convenient deployment method is `rsync`.

Example:

```bash
rsync -avz -e "ssh -i devops-ec2-key.pem" ./website/ ubuntu@<EC2_PUBLIC_IP>:/tmp/website/
```

Then:

```bash
ssh -i devops-ec2-key.pem ubuntu@<EC2_PUBLIC_IP>
```

Deploy:

```bash
sudo cp -r /tmp/website/* /var/www/html/
```

This becomes useful later when creating CI/CD pipelines.

---

# 🚀 Stretch Goal 1: Custom Domain

You can connect a domain to the EC2 server.

Architecture:

```text
Domain
   |
   v
DNS
   |
   v
EC2 Public IP
   |
   v
Nginx
   |
   v
Website
```

Create an appropriate DNS `A` record pointing your domain to the EC2 public IP.

If using Amazon Route 53, you can manage the DNS hosted zone there.

> For production, consider using an Elastic IP or another stable endpoint rather than relying on an automatically assigned public IPv4 address.

---

# 🔒 Stretch Goal 2: HTTPS

For HTTPS, you can use:

* Let's Encrypt
* Certbot
* AWS Certificate Manager with an appropriate AWS architecture

For a simple single-server Nginx deployment, Certbot can be used with Let's Encrypt.

Example installation:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

Then, after your domain correctly points to the server:

```bash
sudo certbot --nginx
```

Verify:

```bash
sudo certbot certificates
```

---

# 🔄 Stretch Goal 3: CI/CD

A simple future CI/CD architecture:

```text
Developer
    |
    v
GitHub
    |
    v
GitHub Actions
    |
    v
Build / Test
    |
    v
SSH / rsync
    |
    v
AWS EC2
    |
    v
Nginx
    |
    v
Static Website
```

A deployment pipeline could:

1. Checkout code
2. Test website
3. Connect to EC2
4. Copy files
5. Validate Nginx
6. Reload Nginx

Example deployment command:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

# 🔐 AWS Security Best Practices

For a real environment:

### SSH

Avoid:

```text
0.0.0.0/0 → TCP 22
```

Prefer:

```text
Your IP → TCP 22
```

### HTTP

For a public website:

```text
0.0.0.0/0 → TCP 80
```

may be appropriate.

### Private Keys

Never commit:

```text
*.pem
*.key
id_rsa
```

to GitHub.

Example `.gitignore`:

```gitignore
*.pem
*.key
.env
id_rsa
id_ed25519
```

### IAM

Avoid using the AWS root account for everyday operations.

Use an appropriate IAM identity with only the permissions required for the task.

---

# 💰 Cost Awareness

EC2 instances can incur charges depending on your account, region, instance type, storage, public IPv4 usage, and other resources.

Before starting:

* Check current AWS pricing
* Check current Free Tier eligibility
* Stop or terminate resources when finished
* Remove unused EBS volumes
* Remove unused Elastic IPs/resources
* Monitor billing

When you are completely finished with the lab, terminate the instance if you no longer need it.

---

# 🧪 Project Validation

Use this checklist:

```bash
# SSH
ssh -i devops-ec2-key.pem ubuntu@<EC2_PUBLIC_IP>

# OS
cat /etc/os-release

# Nginx
sudo systemctl status nginx

# Nginx configuration
sudo nginx -t

# Website
curl http://localhost

# Port 80
sudo ss -lntp | grep :80

# Logs
sudo tail -n 20 /var/log/nginx/access.log
```

Finally open:

```text
http://<EC2_PUBLIC_IP>
```

---

# 📚 Learning Outcomes

After completing this project, you should understand:

* AWS EC2
* AMIs
* Instance types
* VPC basics
* Subnets
* Security Groups
* Public and private IP addresses
* SSH authentication
* Linux server administration
* Nginx installation
* Nginx configuration
* Static website deployment
* Linux services
* Basic AWS security

---

# ❓ Interview Questions

### 1. What is EC2?

EC2 is an AWS service that provides virtual servers in the cloud.

### 2. What is an AMI?

An AMI is a template containing the information required to launch an EC2 instance.

### 3. What is a Security Group?

A Security Group is a stateful virtual firewall that controls network traffic for resources such as EC2 instances.

### 4. What is the difference between public and private IP?

A public IP can be used for internet communication, while a private IP is used for communication inside the VPC/private network.

### 5. How do you connect to Ubuntu EC2?

Example:

```bash
ssh -i key.pem ubuntu@<PUBLIC_IP>
```

### 6. Why do we use port 22?

Port 22 is commonly used by SSH.

### 7. Why do we use port 80?

Port 80 is commonly used for HTTP traffic.

### 8. How do you check Nginx status?

```bash
sudo systemctl status nginx
```

### 9. How do you test Nginx configuration?

```bash
sudo nginx -t
```

### 10. Where are Nginx logs?

Common locations:

```text
/var/log/nginx/access.log
/var/log/nginx/error.log
```

### 11. What happens if the EC2 public IP changes?

If you are using the automatically assigned public IPv4 address, it can change after certain stop/start operations. For a stable public endpoint, use an Elastic IP or another appropriate AWS networking architecture.

### 12. How would you make this deployment production-ready?

Possible improvements include:

* Elastic IP or load balancer
* HTTPS
* Route 53
* CI/CD
* IAM least privilege
* Monitoring
* CloudWatch
* Backups
* Automated configuration management
* Auto Scaling
* Infrastructure as Code with Terraform

---

# 🏢 Production Evolution

This beginner project can grow into a production DevOps architecture.

### Beginner

```text
Internet
   |
   v
EC2
   |
   v
Nginx
   |
   v
Static Website
```

### Intermediate

```text
Route 53
   |
   v
EC2
   |
   v
Nginx
   |
   v
Application
```

### Advanced

```text
                    Route 53
                        |
                        v
                   Load Balancer
                        |
              +---------+---------+
              |                   |
              v                   v
            EC2                  EC2
              |                   |
              +---------+---------+
                        |
                        v
                   Application
                        |
             +----------+----------+
             |                     |
             v                     v
          Database             Monitoring
                                  |
                                  v
                           CloudWatch/Grafana
```

### DevOps CI/CD Version

```text
Developer
    |
    v
GitHub
    |
    v
GitHub Actions
    |
    +---- Build
    |
    +---- Test
    |
    +---- Security Scan
    |
    +---- Deploy
    |
    v
AWS
    |
    v
EC2 / Load Balancer
    |
    v
Application
```

---

# 📋 Final Checklist

* [ ] AWS account created
* [ ] EC2 service opened
* [ ] Ubuntu AMI selected
* [ ] `t2.micro` selected if appropriate
* [ ] Key pair created/downloaded
* [ ] Default VPC/subnet selected
* [ ] Public IP enabled
* [ ] Security Group configured
* [ ] Port 22 allowed
* [ ] Port 80 allowed
* [ ] EC2 instance launched
* [ ] SSH connection successful
* [ ] Ubuntu updated
* [ ] Nginx installed
* [ ] Nginx enabled
* [ ] `index.html` created
* [ ] Website deployed
* [ ] Website tested with `curl`
* [ ] Website opened using public IP
* [ ] Nginx logs checked
* [ ] Private key protected
* [ ] AWS resources stopped/terminated after testing

---

# 📤 GitHub Upload

Initialize Git:

```bash
git init
```

Add files:

```bash
git add README.md index.html
```

Commit:

```bash
git commit -m "Add AWS EC2 static website project"
```

Create the main branch:

```bash
git branch -M main
```

Add your repository:

```bash
git remote add origin https://github.com/<YOUR_USERNAME>/awsec2linuxdevops.git
```

Push:

```bash
git push -u origin main
```

---

# 📌 GitHub Repository Description

```text
Beginner AWS DevOps project demonstrating how to launch an Ubuntu EC2 instance, connect using SSH, configure security groups, install Nginx, and deploy a static website.
```

## 🏷️ GitHub Topics

```text
aws
ec2
linux
devops
nginx
ssh
cloud
aws-ec2
static-website
cloud-computing
```

---

# 🏁 Conclusion

This project provides a practical foundation for AWS and DevOps.

You launched an EC2 Linux server, configured network access using a Security Group, connected through SSH, installed Nginx, and deployed a static website.

The natural next progression is:

```text
AWS EC2
   ↓
Linux Administration
   ↓
Nginx
   ↓
Custom Domain
   ↓
HTTPS
   ↓
GitHub Actions CI/CD
   ↓
Terraform
   ↓
Load Balancer
   ↓
Auto Scaling
   ↓
CloudWatch Monitoring
   ↓
Production AWS DevOps
```

