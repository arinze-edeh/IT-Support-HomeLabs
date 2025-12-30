# 03 – Active Directory Lab (Part 2)

## 📌 Overview
Configured **Group Policies, permissions, login scripts, and user accounts**.

---

## 🛠️ Tools Used
- GPMC (Group Policy Management Console)
- PowerShell

---

## 🧩 Steps Performed

### 1. Created User Accounts
📸 *Screenshot: Users created*
<img width="958" height="792" alt="image" src="https://github.com/user-attachments/assets/6620b1af-ad4d-4dba-86c5-9412eb28f226" />


---

### 2. Created Group Policies
📸 *Screenshot: GPO list*
<img width="990" height="789" alt="image" src="https://github.com/user-attachments/assets/ea4f1ef1-ec78-4c83-8c7c-de5f5234c710" />

---

### 3. Linked GPOs to OUs
📸 *Screenshot: GPO link result*
<img width="997" height="894" alt="Screenshot054628" src="https://github.com/user-attachments/assets/1924411e-2d81-4968-bf0a-66fa8ea39154" />

<img width="988" height="775" alt="image" src="https://github.com/user-attachments/assets/d267af55-f263-478d-b69e-39669c10b319" />

---

### 4. Applied Login Scripts
📸 *Screenshot: Script path*
<img width="967" height="845" alt="Screenshot044122" src="https://github.com/user-attachments/assets/b2aec2a2-0823-4604-af9b-1c68d1e0d851" />

<img width="961" height="786" alt="Screenshot020444" src="https://github.com/user-attachments/assets/9db92d52-ef12-4d4c-bd3f-b67fcade7df5" />

---

### 5. Tested Policies with Client VM
📸 *Screenshot: Resulting policy applied*
<img width="1020" height="891" alt="Screenshot025235" src="https://github.com/user-attachments/assets/49882850-ccf0-4c71-8610-414a118249a5" />
<img width="1030" height="774" alt="Screenshot060858" src="https://github.com/user-attachments/assets/b7f0998d-c0fa-4b04-bbda-e0b097d276b6" />

<img width="988" height="766" alt="Screenshot055852" src="https://github.com/user-attachments/assets/ddb0d9dd-73c1-4716-8411-66dbef6f9f45" />

<img width="969" height="515" alt="Screenshot020315" src="https://github.com/user-attachments/assets/b634a310-1a79-480d-9f5e-ab3949be81fd" />


---

## 📘 What I Learned

- How to join a Windows client machine to an Active Directory domain.
- The importance of correct DNS configuration for domain authentication and Group Policy.
- How Active Directory uses DNS to resolve domain services.
- How to create and apply Group Policy Objects (GPOs) to Organizational Units.
- How to verify Group Policy application using `gpupdate /force` and `gpresult /r`.
- How to troubleshoot domain login issues related to password resets.
- The difference between local user authentication and domain user authentication.
- How to diagnose and fix client-side Group Policy issues.
- How domain-level Group Policies override local Group Policy settings.
- The importance of proper network configuration in virtualized lab environments.
  

---

## ❗ Issues & Fixes

### 1. Client VM Not Joined to Domain
**Issue:** Windows 10 client displayed `WORKGROUP` instead of the domain.  
**Fix:**  
- Joined the client to `arinzelab.local` using domain administrator credentials.  
- Restarted the system to complete domain membership.

---

### 2. Domain Name Not Resolving
**Issue:** `ping arinzelab.local` failed with “host not found.”  
**Fix:**  
- Configured the client’s DNS to point to the Domain Controller.  
- Cleared the DNS cache using `ipconfig /flushdns`.

---

### 3. DHCP Renewal Failure
**Issue:** `ipconfig /renew` failed with a DHCP timeout error.  
**Fix:**  
- Ensured both client and server were on the same internal network.  
- Verified the DHCP service was running on the Domain Controller.

---

### 4. Domain User Login Failure After Password Reset
**Issue:** User could not log in after a forced password change.  
**Fix:**  
- Reset the password without enforcing an immediate change.  
- Logged in using the correct format: `arinzelab\jdoe`.

---

### 5. Group Policy Not Applying
**Issue:** Group Policy Objects appeared not to apply to the client system.  
**Fix:**  
- Ran `gpresult /r` to confirm policy application.  
- Verified that **Disable Control Panel** and **LoginScript-GPO** were applied.


---

## ✅ Final Outcome
Successfully joined the domain, authenticated domain users correctly, and applied Group Policy Objects as expected.

