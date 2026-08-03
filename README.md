https://roadmap.sh/projects/ec2-instance
# EC2-Instance
# AWS EC2 Static Website Deployment

A beginner-friendly DevOps project demonstrating how to launch an Amazon EC2 Linux instance, configure security groups, connect via SSH, and host a static web application using Nginx.

---

## 📌 Project Overview

The objective of this project is to set up a cloud-hosted web server on Amazon Web Services (AWS) using an EC2 instance, configure network access, and deploy a simple HTML static website.

### Objectives
- Create and navigate an AWS Account.
- Provision a Linux Virtual Machine (`t2.micro` Ubuntu Instance).
- Configure Security Groups (Ports 22 for SSH & 80 for HTTP).
- Connect securely to the server via SSH.
- Install and configure Nginx web server.
- Deploy a static web application reachable via public IP.

---

## 🛠️ Requirements & Tools

- **Cloud Provider:** Amazon Web Services (AWS)
- **OS / Machine Image:** Ubuntu Server (Latest LTS)
- **Instance Type:** `t2.micro` (AWS Free Tier Eligible)
- **Web Server:** Nginx
- **Terminal Client:** SSH (Linux/macOS) or PowerShell / PuTTY (Windows)

---

## 🚀 Step-by-Step Implementation Guide

### Step 1: Launch an EC2 Instance
1. Log in to the [AWS Management Console](https://aws.amazon.com/console/).
2. Navigate to **EC2** and click **Launch Instance**.
3. **Name:** `Static-Web-Server` (or your choice).
4. **Application and OS Images (AMI):** Select **Ubuntu** (Free Tier eligible).
5. **Instance Type:** Select `t2.micro`.
6. **Key Pair:**
   - Click **Create new key pair**.
   - Key pair name: `my-ec2-key`
   - Key pair type: `RSA`, Private key file format: `.pem` (or `.ppk` for PuTTY).
   - Download and safely save the `.pem` file.

### Step 2: Configure Network & Security Group
1. Under **Network settings**:
   - Ensure **Auto-assign Public IP** is set to **Enable**.
2. **Create Security Group:**
   - Allow **SSH** traffic: `Port 22` | Source: `My IP` (Recommended) or `0.0.0.0/0`.
   - Allow **HTTP** traffic: `Port 80` | Source: `Anywhere (0.0.0.0/0)`.
3. Review configurations and click **Launch Instance**.

---

### Step 3: Connect to the Server via SSH
1. Open your terminal or command line interface.
2. Set file permissions for your downloaded private key (macOS/Linux):
   ```bash
   chmod 400 my-ec2-key.pem
