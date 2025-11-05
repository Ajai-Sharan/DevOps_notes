# ☁️ AWS & 🐧 Linux Introduction — Notes

---

## 🌍 AWS Fundamentals

### 🗺️ AWS Regions – The Global Layer

**Definition:**  
An **AWS Region** is a **geographically isolated area** containing multiple **Availability Zones (AZs)**.  
Each region is independent — ensuring **data sovereignty, fault tolerance, and low latency**.

| **Region Name** | **Code** | **Location** |
|------------------|-----------|---------------|
| US East (N. Virginia) | `us-east-1` | North America |

> Each region has its own resources — EC2, RDS, S3, etc.

---

### 🏢 Availability Zones (AZs)

- Each region has **2–6 AZs**.  
- An **AZ** = one or more **physically separate datacenters** connected via **low-latency links**.

**Example:**
Region: ap-south-1
├── ap-south-1a
├── ap-south-1b
└── ap-south-1c


---

### 🌐 VPC (Virtual Private Cloud) Overview

A **VPC** is an **isolated virtual network** inside a region for your AWS resources.

**You can create multiple VPCs in a region.**

**You can define:**
- IP address ranges (CIDR blocks)
- Subnets
- Route tables
- Internet Gateways
- Security groups / NACLs

---

### 🧱 Subnets in AWS

**Definition:**  
A **Subnet** is a **smaller network segment** inside your VPC.  
Each subnet belongs to **one AZ** and divides a VPC into **logical groups (public/private)**.

#### ✅ Subnet Best Practices
1. Use **multiple AZs** for redundancy  
2. Divide subnets into **tiers** (public, private, isolated)  
3. Use **NAT Gateways** for private subnet internet access  
4. Reserve **CIDR space** for scaling  
5. **Tag** subnets clearly (e.g., `Env=Prod`, `Tier=Public`)  
6. Use **Route Tables** & **Security Groups** to isolate traffic  

---

## ⚙️ AWS Fields When Launching EC2 Instances

### 1. 🏷️ Name & Tags
- `Name`: A key-value tag to identify the instance (e.g., `Name=WebServer`)
- Additional tags: `Environment=Production`, `Role=AppServer` for organization & automation.

---

### 2. 💽 Application & OS Images (AMI)
- **AMI** = Amazon Machine Image (contains OS + software)
- Choose OS: Amazon Linux, Ubuntu, Windows  
- **Free Tier eligible** AMIs are cost-free
- Sources: Quick Start, Marketplace, Community AMIs

---

### 3. ⚡ Instance Type
Defines **hardware configuration** — vCPUs, RAM, Network performance.  
Examples: `t3.micro`, `m5.large`  

- Choose based on workload (CPU, memory, or general purpose)
- **Free Tier:** usually limited to `t2.micro` or `t3.micro`

---

### 4. 🔑 Key Pair (Login)
Used for **SSH (Linux)** or **RDP (Windows)** access.  
If skipped, you may lose secure access.

---

### 5. 🌐 Network Settings
Defines how your instance connects to the network.

Includes:
- **VPC/Subnet:** choose where to launch  
- **Auto-assign Public IP:** allows internet access  
- **Security Groups:** inbound/outbound firewall rules  
- **Network Interfaces:** attach multiple interfaces if needed  

Subnet type determines accessibility (public/private).

---

### 6. 💾 Configure Storage
Each instance has a **root volume** (from AMI) + optional **EBS volumes**.

- **Volume Types:** `gp3`, `gp2`, `io1`, `io2`, `st1`, etc.  
- **Size (GiB)** — define storage size  
- **IOPS / Throughput** — control performance for high-speed disks  
- **Delete on Termination** — auto-delete on termination  
- **Encryption** — encrypt using AWS KMS  
- **Device Name** — appears as `/dev/xvda`, `/dev/sdb`, etc.  

---

### 7. ⚙️ Advanced Details
Includes **IAM, monitoring, user data, and system settings**.

| **Field** | **Purpose** |
|------------|-------------|
| IAM instance profile | Assigns permissions (e.g., S3 access) |
| Shutdown behavior | Stop or terminate instance on shutdown |
| Termination protection | Prevent accidental deletions |
| Monitoring | Enable detailed CloudWatch (1-min intervals) |
| Placement group | Cluster, partition, or spread instances |
| Tenancy | Shared or dedicated hardware |
| User data | Startup scripts (install packages, configs) |
| Boot mode | UEFI vs Legacy BIOS |
| Metadata options | Configure Instance Metadata Service (IMDSv2) |
| License options | Specify OS/software licenses |
| Tag specifications | Apply tags to instance, volumes, etc. |

---

## 🐧 Linux Fundamentals

### 🧩 What is Linux?
An **open-source operating system** used in servers, cloud systems, DevOps, and automation.

---

### 🏗️ Linux Architecture

**Linux Kernel**
- Manages **CPU, memory, devices, filesystems**
- Provides **system calls** for apps to interact with hardware
- Handles **process scheduling**, **networking**, and **security**

**Shell**
- Command-line interface between user & kernel  
- Common shells: `bash`, `zsh`, `sh`, `fish`

**Why Learn Shell Commands?**
- Faster automation  
- DevOps scripting foundation  
- Easier debugging & navigation  

---

## 🔤 1. Basic Linux Commands

| **Command** | **Purpose** | **Example** |
|--------------|-------------|--------------|
| `pwd` | Print working directory | `pwd` |
| `cd` | Change directory | `cd /home/user/Documents` |
| `mkdir` | Create directories | `mkdir projects` |
| `rmdir` | Remove empty directories | `rmdir old_folder` |
| `touch` | Create empty files | `touch file1.txt` |
| `cat` | Display or concatenate files | `cat file1.txt` |
| `echo` | Print or write text | `echo "Hello" > greet.txt` |
| `history` | Show command history | `history` |

**🧪 Lab 1 — Basic Commands**
1. Create `starting_point` directory  
2. Navigate inside it  
3. Create `secret_lair.txt`  
4. Show absolute path  
5. View command history  

**Expected Outcome:** Create, navigate, and manipulate files/directories.

---

## 📁 2. File Management Commands

| **Command** | **Purpose** | **Example** |
|--------------|-------------|--------------|
| `ls` | List files | `ls -lh` |
| `cp` | Copy files | `cp file1.txt /backup/` |
| `mv` | Move/Rename files | `mv old.txt new.txt` |
| `rm` | Remove files | `rm -r temp_folder` |
| `find` | Search for files | `find . -name "*.log"` |
| `du` | Show disk usage | `du -sh /var/log/` |
| `df` | Show free disk space | `df -h` |

**🧪 Lab 2 — File Management**
- Delete directories/files safely  
- Use `find` to locate `.txt` files  
- Check `/var/log` disk usage  

---

## ⚙️ 3. Process Management Commands

| **Command** | **Purpose** | **Example** |
|--------------|-------------|--------------|
| `ps` | View running processes | `ps -ef` |
| `top` | Real-time monitoring | `top` |
| `htop` | Enhanced top (interactive) | `htop` |
| `kill` | Terminate process by PID | `kill -9 1234` |
| `jobs`, `bg`, `fg` | Manage background/foreground jobs | `fg %1` |

**🧪 Lab 3 — Process Management**
1. List running processes  
2. Find PID of `sshd`  
3. Start background job (`sleep 60 &`)  
4. Bring to foreground  
5. Kill a process by PID  

---

## 🔐 Extended Concept — Permissions & Ownership

| **Command** | **Purpose** | **Example** |
|--------------|-------------|--------------|
| `chmod` | Change permissions | `chmod 755 file.sh` |
| `chown` | Change ownership | `chown user:group file.sh` |
| `chgrp` | Change group ownership | `chgrp dev file.txt` |

**🧪 Lab 4 — Advanced Challenge**
- Work inside `secret_lair`  
- Count subdirectories  
- Identify restricted directory  
- Change ownership to `user`  
- Write all answers to `/home/user/answer.txt`

---

# ✅ Summary

### AWS
- **Region:** isolated global area  
- **AZ:** datacenter cluster  
- **VPC:** private network  
- **Subnet:** divides VPC logically  
- **EC2 setup:** Name, AMI, Type, Key Pair, Network, Storage, Advanced settings  

### Linux
- Open-source OS used in servers  
- Learn **basic**, **file**, and **process** management  
- Essential for **DevOps automation & cloud administration**

---
