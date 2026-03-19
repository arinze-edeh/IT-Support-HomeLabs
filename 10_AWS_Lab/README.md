# Lab 10 - AWS Fundamentals

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Tools and Technologies](#tools-and-technologies)
4. [Lab Configuration Steps](#lab-configuration-steps)
5. [Key Learnings](#key-learnings)
6. [Issues and Fixes](#issues-and-fixes)
7. [Outcome Summary](#outcome-summary)

---

## Overview

This lab covers foundational AWS infrastructure concepts through hands-on deployment and configuration of core cloud services. The focus areas include compute provisioning, object storage, network access control, and identity management, all validated through a live SSH session into a running EC2 instance.

**Scope of work:**
- Deployed core AWS infrastructure components across compute, storage, networking, and access control
- Applied least-privilege principles through IAM user scoping
- Validated secure remote access to a cloud-hosted Linux instance via SSH

---

## Prerequisites

- An active AWS account with sufficient permissions to create IAM users, EC2 instances, S3 buckets, and Security Groups
- A local machine with an SSH client installed (e.g., OpenSSH on Linux/macOS, or PuTTY/Windows Terminal on Windows)
- A downloaded EC2 key pair (.pem file) generated at instance launch
- Basic familiarity with the AWS Management Console

---

## Tools and Technologies

| Tool / Service | Purpose |
|---|---|
| AWS Management Console | Web-based interface for provisioning and managing all AWS resources |
| Amazon EC2 (Elastic Compute Cloud) | Scalable virtual compute instance (Amazon Linux) |
| Amazon S3 (Simple Storage Service) | Object storage provisioning and access management |
| AWS IAM (Identity and Access Management) | User creation and least-privilege permission scoping |
| Security Groups | Virtual firewall for controlling inbound and outbound instance traffic |

---

## Lab Configuration Steps

### 1. Created IAM User

Created an IAM user with scoped permissions to securely manage AWS resources, following least-privilege principles to limit access to only what is necessary.

📸 *Screenshot: IAM User Creation and Permissions*

<img width="1919" height="941" alt="Screenshot 2026-01-03 001809" src="https://github.com/user-attachments/assets/c3f14038-332b-46c9-9b0e-7f8a8a13d1aa" />
<img width="1919" height="946" alt="Screenshot 2026-01-03 001609" src="https://github.com/user-attachments/assets/c212bc96-0181-400b-b223-d29224bd22fc" />

---

### 2. Launched EC2 Instance

Launched an Amazon EC2 instance using the Amazon Linux AMI to provide a scalable, cloud-hosted virtual machine for compute workloads.

📸 *Screenshot: EC2 Instance Running State / Dashboard*

<img width="1914" height="882" alt="image" src="https://github.com/user-attachments/assets/2107b2ad-4338-4c86-b62d-f7ca9e609126" />
<img width="1915" height="926" alt="image" src="https://github.com/user-attachments/assets/b257af8d-359f-4c34-9d9d-0325a6638c15" />

---

### 3. Configured Security Groups

Configured Security Group inbound rules to allow controlled access on port 22 (SSH) and port 80 (HTTP), restricting traffic to authorized sources only.

📸 *Screenshot: Security Group Inbound Rules*

<img width="1136" height="948" alt="Screenshot 2026-01-03 010458" src="https://github.com/user-attachments/assets/3cb27a11-78fe-4fee-b5c4-bee7a306582f" />
<img width="1919" height="933" alt="Screenshot 2026-01-03 010534" src="https://github.com/user-attachments/assets/0aa80c93-b719-4799-9f93-4e2edea4afd4" />

---

### 4. Created S3 Bucket

Provisioned an Amazon S3 bucket to demonstrate object storage creation, access policy configuration, and public access controls.

📸 *Screenshot: S3 Bucket*

<img width="1145" height="926" alt="image" src="https://github.com/user-attachments/assets/78d7305f-4074-4bd8-a1e8-9d0e151e15a0" />
<img width="1915" height="928" alt="image" src="https://github.com/user-attachments/assets/89f2f5ed-0e92-41d6-9717-fc10cbd51fdb" />

---

### 5. Connected to EC2 via SSH

Successfully established an SSH session to the running EC2 instance, confirming remote access and validating that the instance, Security Group rules, and key pair were all correctly configured.

📸 *Screenshot: EC2 SSH Session (Amazon Linux)*

<img width="1919" height="927" alt="Screenshot 2026-01-03 013057" src="https://github.com/user-attachments/assets/0df1606a-9ffd-4a12-b272-961f1edea535" />

---

## Key Learnings

- IAM users should be created with the minimum permissions needed; least-privilege access reduces the attack surface significantly
- EC2 instances require a correctly configured key pair at launch; the private key cannot be retrieved afterward
- Security Groups act as stateful firewalls; inbound rules must explicitly permit the required ports and protocols
- SSH (port 22) is the correct protocol for Linux-based EC2 instances; RDP applies to Windows instances only
- S3 buckets default to blocking all public access; permissions must be deliberately adjusted for any intended public exposure
- Sensitive data such as public IP addresses should be redacted in screenshots before being shared publicly

---

## Issues and Fixes

**SSH connection blocked**
The EC2 instance was unreachable via SSH on initial attempt. The Security Group inbound rules had not been configured to permit TCP traffic on port 22. Adding an inbound rule scoped to the local IP address resolved the issue.

**Public access restrictions on S3 bucket**
The S3 bucket was created with default public access block settings enabled. Bucket permissions and access policies were reviewed and adjusted to match the intended access level.

**Confusion between RDP and SSH**
Initial uncertainty arose over which remote access protocol to use. Confirmed that Linux-based EC2 instances use SSH, while Windows instances use RDP.

**Sensitive data in screenshots**
Public IP addresses were visible in several screenshots. All affected images were reviewed and IP addresses were redacted prior to uploading to GitHub.

---

## Outcome Summary

| Task | Status |
|---|---|
| IAM user created with least-privilege permissions | Complete |
| EC2 instance launched and running (Amazon Linux) | Complete |
| Security Group rules configured for SSH and HTTP | Complete |
| S3 bucket provisioned and access policy verified | Complete |
| SSH session established and remote access validated | Complete |

All core AWS fundamentals were successfully deployed, configured, and verified within this lab. The environment demonstrated practical application of compute, storage, networking, and identity and access management concepts on AWS.
