# 09 – Azure Fundamentals Lab

## 📌 Overview

- Deployed core Azure infrastructure components using the Azure Portal
- Implemented compute, networking, storage, and access control services
- Validated resource deployment and permissions within a constrained student subscription

---

## 🛠️ Tools Used

- Microsoft Azure Portal
- Azure Resource Groups
- Azure Virtual Machines
- Azure Virtual Network (VNet)
- Network Security Groups (NSGs)
- Azure Storage Account
- Azure Role-Based Access Control (RBAC)

---

## 🧩 Steps Performed

### 1. Created Resource Group

Created an Azure Resource Group to logically organize and manage all lab resources within a single lifecycle and region.

📸 *Screenshot: resource group*

<img width="1919" height="956" alt="image" src="https://github.com/user-attachments/assets/6b0c69d0-c9bc-4f16-a6ea-2e67fcc339e9" />

---

### 2. Deployed VM

Deployed an Azure virtual machine using the Azure Portal, configuring compute size, networking, and OS image for testing and management tasks.

📸 *Screenshot: VM deployment*

<img width="1919" height="954" alt="image" src="https://github.com/user-attachments/assets/fce1d470-08d4-4da0-ba73-eed792ee8025" />
<img width="1919" height="950" alt="image" src="https://github.com/user-attachments/assets/8e0d2b83-b97f-4061-98d7-041cef1ca686" />
<img width="1919" height="955" alt="image" src="https://github.com/user-attachments/assets/7732c888-b310-45b0-9bbc-35ddcc951990" />
<img width="1919" height="955" alt="image" src="https://github.com/user-attachments/assets/05fb313d-ecfd-4d13-8282-02151ff19b24" />
<img width="1919" height="953" alt="image" src="https://github.com/user-attachments/assets/1f7da0f9-a960-40e7-b3c7-46d074755b83" />

---

### 3. Configured VNet & NSGs

A Virtual Network and subnet were automatically created during VM deployment via the Networking configuration. Network Security Group rules were later customized to allow inbound traffic on port 8080.

📸 *Screenshot: NSG rules*

<img width="1126" height="949" alt="Screenshot 2026-01-02 153734" src="https://github.com/user-attachments/assets/e2aa982e-299c-4225-a779-d7c13609ede6" />
<img width="1125" height="949" alt="Screenshot 2026-01-02 153749" src="https://github.com/user-attachments/assets/4a45cf5a-fd97-42e6-9a63-f51ac4e29559" />
<img width="1919" height="949" alt="image" src="https://github.com/user-attachments/assets/7c861657-4f3f-4d0a-ba59-191e8cb1ee03" />


---

### 4. Created Azure Storage Account

Created an Azure Storage Account to provision scalable cloud storage and verified successful deployment via the storage account dashboard.

📸 *Screenshot: storage dashboard*
<img width="1148" height="952" alt="Screenshot 2026-01-02 155336" src="https://github.com/user-attachments/assets/ad7a7d89-5196-44c8-9860-8b0d759406a3" />
<img width="1149" height="950" alt="Screenshot 2026-01-02 155412" src="https://github.com/user-attachments/assets/1fd23c19-8e21-45c0-bcd9-686fc924d2b6" />
<img width="1919" height="955" alt="image" src="https://github.com/user-attachments/assets/b7e36005-d12f-43d1-a335-c851c8bb183a" />

---

### 5. Configured IAM Roles

IAM roles were successfully configured at the resource group scope. The signed-in Azure user was assigned the Owner role, validating role assignment and access control configuration.

📸 *Screenshot: IAM settings*

<img width="1919" height="953" alt="image" src="https://github.com/user-attachments/assets/e2687a63-b893-4175-bd2f-2de018945be8" />
<img width="1917" height="949" alt="Screenshot 2026-01-02 162548" src="https://github.com/user-attachments/assets/95d9d1c2-4557-47de-a151-42a43dfb235c" />

---

## 📘 What I Learned

- How Azure Resource Groups control resource organization and lifecycle
- How to deploy and configure virtual machines in Azure
- How VNets and NSGs control network traffic and security
- How Azure Storage Accounts provide scalable cloud storage
- How IAM (RBAC) enforces access control at subscription and resource levels 

---

## ❗ Issues & Fixes

- **Issue: Deployment blocked by subscription region policy**
   - Fix: Selected an Azure region allowed by the subscription policy

- **Issue: VM size unavailable**
   - Fix: Chose an alternative supported VM size

- **Issue: Network traffic blocked**
   - Fix: Configured NSG inbound rules to allow required traffic

- **Issue: IAM role assignment validation uncertainty**
   - Fix: Verified role assignments via IAM and Check Access

---

## ✅ Final Outcome

Hands-on Azure foundational experience.
- Resource Group successfully created
- Virtual Machine deployed and operational
- Virtual Network and NSG configured correctly
- Azure Storage Account provisioned
- IAM roles verified and validated
- Azure fundamentals successfully implemented and documented
