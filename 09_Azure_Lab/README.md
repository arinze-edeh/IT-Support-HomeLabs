# 09 Azure Fundamentals Lab

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Tools and Technologies](#tools-and-technologies)
4. [Lab Configuration Steps](#lab-configuration-steps)
   - [1. Create Resource Group](#1-create-resource-group)
   - [2. Deploy Virtual Machine](#2-deploy-virtual-machine)
   - [3. Configure VNet and NSGs](#3-configure-vnet-and-nsgs)
   - [4. Create Azure Storage Account](#4-create-azure-storage-account)
   - [5. Configure IAM Roles](#5-configure-iam-roles)
5. [Key Learnings](#key-learnings)
6. [Issues and Fixes](#issues-and-fixes)
7. [Outcome Summary](#outcome-summary)

---

## Overview

This lab covers the deployment and configuration of core Azure infrastructure components using the Azure Portal. It walks through provisioning compute, networking, storage, and identity/access management services, and validates resource deployment and permissions within a constrained student subscription.

---

## Prerequisites

Before starting this lab, ensure the following are in place:

- An active Microsoft Azure account with a valid student or trial subscription
- Access to the Azure Portal at [portal.azure.com](https://portal.azure.com)
- Sufficient subscription permissions to create and manage resource groups, virtual machines, storage accounts, and IAM role assignments
- A chosen Azure region that is permitted under the subscription policy (some regions may be restricted)
- Basic familiarity with cloud infrastructure concepts (compute, networking, storage, access control)

---

## Tools and Technologies

| Tool / Service | Purpose |
|---|---|
| Microsoft Azure Portal | Primary interface for resource provisioning and management |
| Azure Resource Groups | Logical container for organizing and managing lab resources |
| Azure Virtual Machines | Cloud-hosted compute instance for testing and management |
| Azure Virtual Network (VNet) | Private network and subnet for VM communication |
| Network Security Groups (NSGs) | Firewall rules controlling inbound and outbound traffic |
| Azure Storage Account | Scalable cloud storage provisioning |
| Azure RBAC (IAM) | Role-based access control for resource-level permissions |

---

## Lab Configuration Steps

### 1. Create Resource Group

A Resource Group was created to logically organize and manage all lab resources under a single lifecycle and Azure region. This is the foundational container for all subsequent resources deployed in the lab.

Screenshots: Resource Group creation

<img width="1919" height="956" alt="Resource Group" src="https://github.com/user-attachments/assets/6b0c69d0-c9bc-4f16-a6ea-2e67fcc339e9" />

---

### 2. Deploy Virtual Machine

An Azure Virtual Machine was deployed via the Azure Portal with a configured compute size, OS image, and networking settings. The VM was scoped to the previously created Resource Group and configured for testing and remote management tasks.

Screenshots: VM deployment

<img width="1919" height="954" alt="VM Deployment 1" src="https://github.com/user-attachments/assets/fce1d470-08d4-4da0-ba73-eed792ee8025" />
<img width="1919" height="950" alt="VM Deployment 2" src="https://github.com/user-attachments/assets/8e0d2b83-b97f-4061-98d7-041cef1ca686" />
<img width="1919" height="955" alt="VM Deployment 3" src="https://github.com/user-attachments/assets/7732c888-b310-45b0-9bbc-35ddcc951990" />
<img width="1919" height="955" alt="VM Deployment 4" src="https://github.com/user-attachments/assets/05fb313d-ecfd-4d13-8282-02151ff19b24" />
<img width="1919" height="953" alt="VM Deployment 5" src="https://github.com/user-attachments/assets/1f7da0f9-a960-40e7-b3c7-46d074755b83" />

---

### 3. Configure VNet and NSGs

A Virtual Network and subnet were automatically provisioned during the VM deployment's Networking configuration step. After deployment, Network Security Group rules were manually customized to allow inbound traffic on port 8080, enabling the required application-layer access.

Screenshots: NSG rules

<img width="1126" height="949" alt="NSG Rules 1" src="https://github.com/user-attachments/assets/e2aa982e-299c-4225-a779-d7c13609ede6" />
<img width="1125" height="949" alt="NSG Rules 2" src="https://github.com/user-attachments/assets/4a45cf5a-fd97-42e6-9a63-f51ac4e29559" />
<img width="1919" height="949" alt="NSG Rules 3" src="https://github.com/user-attachments/assets/7c861657-4f3f-4d0a-ba59-191e8cb1ee03" />

---

### 4. Create Azure Storage Account

An Azure Storage Account was provisioned to provide scalable cloud storage within the lab environment. Successful deployment was verified through the storage account dashboard in the Azure Portal.

Screenshots: Storage Account dashboard

<img width="1148" height="952" alt="Storage Account 1" src="https://github.com/user-attachments/assets/ad7a7d89-5196-44c8-9860-8b0d759406a3" />
<img width="1149" height="950" alt="Storage Account 2" src="https://github.com/user-attachments/assets/1fd23c19-8e21-45c0-bcd9-686fc924d2b6" />
<img width="1919" height="955" alt="Storage Account 3" src="https://github.com/user-attachments/assets/b7e36005-d12f-43d1-a335-c851c8bb183a" />

---

### 5. Configure IAM Roles

IAM roles were configured at the Resource Group scope using Azure Role-Based Access Control (RBAC). The signed-in Azure user was assigned the Owner role, and the assignment was validated using the IAM panel and the "Check Access" feature to confirm proper access control configuration.

Screenshots: IAM configuration

<img width="1919" height="953" alt="IAM Settings 1" src="https://github.com/user-attachments/assets/e2687a63-b893-4175-bd2f-2de018945be8" />
<img width="1917" height="949" alt="IAM Settings 2" src="https://github.com/user-attachments/assets/95d9d1c2-4557-47de-a151-42a43dfb235c" />

---

## Key Learnings

- **Resource Groups** serve as the foundational organizational unit in Azure, grouping resources by lifecycle and region for easier management and cleanup
- **Azure Virtual Machines** require coordinated configuration across compute, networking, and storage settings; subscription policies can restrict available regions and VM sizes
- **VNets and NSGs** work together to define the network boundary and enforce traffic rules; NSG inbound/outbound rules must be explicitly configured for non-default ports
- **Azure Storage Accounts** provide a flexible, scalable storage layer that can be provisioned and verified independently of compute resources
- **Azure RBAC (IAM)** allows fine-grained access control at the subscription, resource group, or individual resource scope; the "Check Access" tool is useful for verifying role assignment correctness

---

## Issues and Fixes

| Issue | Fix Applied |
|---|---|
| Deployment blocked by subscription region policy | Selected an Azure region permitted under the subscription policy |
| Target VM size unavailable in selected region | Chose a supported alternative VM size |
| Network traffic blocked after VM deployment | Configured NSG inbound rules to permit traffic on the required port |
| Uncertainty around IAM role assignment validity | Verified role assignments using the IAM panel and the "Check Access" feature |

---

## Outcome Summary

All core Azure infrastructure components were successfully deployed and validated within the student subscription environment.

| Component | Status |
|---|---|
| Resource Group | Created and scoped correctly |
| Virtual Machine | Deployed and operational |
| Virtual Network and NSG | Configured with custom inbound rules |
| Azure Storage Account | Provisioned and verified |
| IAM Role Assignment | Assigned and validated via Check Access |

This lab established hands-on foundational experience with Azure's core services, covering the full lifecycle from resource organization and compute provisioning to network security, storage, and identity-based access control.
