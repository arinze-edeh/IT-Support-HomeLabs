# 10 – AWS Fundamentals Lab

## 📌 Overview

- Deployed core AWS infrastructure components
- Practiced compute, storage, networking, and access control fundamentals
- Validated secure remote access to cloud resources

---

## 🛠️ Tools Used

- AWS Management Console
- Amazon EC2 (Elastic Compute Cloud)
- Amazon S3 (Simple Storage Service)
- AWS IAM (Identity and Access Management)
- Security Groups (Virtual Firewall)

---

## 🧩 Steps Performed

### 1. Created IAM User

Created an IAM user with scoped permissions to securely manage AWS resources following least-privilege principles.

📸 *Screenshot: IAM User Creation & Permissions*

<img width="1919" height="941" alt="Screenshot 2026-01-03 001809" src="https://github.com/user-attachments/assets/c3f14038-332b-46c9-9b0e-7f8a8a13d1aa" />
<img width="1919" height="946" alt="Screenshot 2026-01-03 001609" src="https://github.com/user-attachments/assets/c212bc96-0181-400b-b223-d29224bd22fc" />

---

### 2. Launched EC2 Instance

Launched an Amazon EC2 instance using Amazon Linux to provide scalable cloud compute resources.

📸 *Screenshot: EC2 Instance Running State / dashboard*

<img width="1914" height="882" alt="image" src="https://github.com/user-attachments/assets/2107b2ad-4338-4c86-b62d-f7ca9e609126" />
<img width="1915" height="926" alt="image" src="https://github.com/user-attachments/assets/b257af8d-359f-4c34-9d9d-0325a6638c15" />

---

### 3. Configured Security Groups

Configured Security Group inbound rules to allow controlled SSH (port 22) and HTTP (port 80) access.

📸 *Screenshot: Security Group Inbound Rules*

<img width="1136" height="948" alt="Screenshot 2026-01-03 010458" src="https://github.com/user-attachments/assets/3cb27a11-78fe-4fee-b5c4-bee7a306582f" />
<img width="1919" height="933" alt="Screenshot 2026-01-03 010534" src="https://github.com/user-attachments/assets/0aa80c93-b719-4799-9f93-4e2edea4afd4" />

---

### 4. Created S3 Bucket

Created an Amazon S3 bucket to demonstrate object storage provisioning and access management.

📸 *Screenshot: S3 bucket*

<img width="1145" height="926" alt="image" src="https://github.com/user-attachments/assets/78d7305f-4074-4bd8-a1e8-9d0e151e15a0" />
<img width="1915" height="928" alt="image" src="https://github.com/user-attachments/assets/89f2f5ed-0e92-41d6-9717-fc10cbd51fdb" />

---

### 5. Connected to EC2 via SSH

Successfully connected to the EC2 instance using SSH to validate remote access and instance readiness.

📸 *Screenshot: connected EC2 SSH Session (Amazon Linux)*

<img width="1919" height="927" alt="Screenshot 2026-01-03 013057" src="https://github.com/user-attachments/assets/0df1606a-9ffd-4a12-b272-961f1edea535" />

---

## 📘 What I Learned

- How to create and manage IAM users with least-privilege permissions
- How to launch and configure an EC2 instance using Amazon Linux
- How Security Groups control inbound traffic using rule-based filtering
- How to create and manage S3 buckets for cloud storage
- How to securely connect to a Linux EC2 instance using SSH 

---

## ❗ Issues & Fixes

- **Issue: SSH connection blocked**
  - Fix: Updated Security Group inbound rules to allow TCP port 22 from my IP

- **Issue: Public access restrictions on S3 bucket**
  - Fix: Adjusted bucket permissions and verified access policies

- **Issue: Confusion between RDP and SSH**
  - Fix: Confirmed Linux EC2 instances require SSH, not RDP

- **Issue: Exposing sensitive data in screenshots**
  - Fix: Redacted public IP addresses before uploading to GitHub

---

## ✅ Final Outcome

- Successfully created an IAM user for controlled AWS access
- Launched and configured a functional EC2 instance
- Applied secure Security Group rules for SSH and HTTP traffic
- Created and verified an S3 storage bucket
- Connected to the EC2 instance via SSH and validated remote access
