# 02 – Active Directory Lab (Part 1)

## 📌 Overview
Set up a **Windows Server Domain Controller**, domain, and organizational units.

---

## 🛠️ Tools Used
- Windows Server 2022
- Active Directory Domain Services (AD DS)

---

## 🧩 Steps Performed

### 1. Installed Windows Server
📸 *Screenshot: Server installation*
<img width="1031" height="899" alt="Screenshot40149" src="https://github.com/user-attachments/assets/38bc2990-fce6-4b6f-be8b-8829347083dc" />


---

### 2. Set Static IP for Server
📸 *Screenshot: Network adapter IPv4 settings*
<img width="957" height="798" alt="Screenshot033139" src="https://github.com/user-attachments/assets/6b86faba-ef50-4260-a48a-986cb7576608" />


---

### 3. Installed Active Directory Domain Services
📸 *Screenshot: AD DS role installation*
<img width="959" height="1074" alt="Screenshot035055" src="https://github.com/user-attachments/assets/e79e3414-ba28-465c-8368-6245e02c50bc" />


---

### 4. Promoted Server to Domain Controller
📸 *Screenshot: Domain name creation*
<img width="960" height="1079" alt="Screenshot035434" src="https://github.com/user-attachments/assets/94112cb3-e047-4021-9a1b-04ae7331b19e" />
<img width="1118" height="1079" alt="Screenshot 042108" src="https://github.com/user-attachments/assets/490310a6-9f65-4cc8-a642-d054b4748410" />

---

### 5. Created OUs (Users, Computers, Admins)
📸 *Screenshot: OU structure*
<img width="1009" height="890" alt="Screenshot044240" src="https://github.com/user-attachments/assets/77af2dfb-0a2c-4e12-b8b9-fc3042c93697" />
<img width="1015" height="890" alt="Screenshot044340" src="https://github.com/user-attachments/assets/21222c6a-de21-472d-a9db-ad8a05c716f2" />
<img width="1129" height="1079" alt="Screenshot045649" src="https://github.com/user-attachments/assets/de038666-4b8f-4485-b240-e6db72deebbd" />


---

## 📘 What I Learned
- Domain controller setup  
- AD DS structure and function  
- OU organization best practices  

---

## ❗ Issues & Fixes Encountered

### 1. Server Not Receiving Internet Access  
**Issue:** After setting a static IP, the server lost internet connection.  
**Fix:**  
- Ensured DNS was set to the correct gateway (e.g., 192.168.56.1).  
- Verified that the NAT adapter was still attached in VirtualBox/VMware settings.

### 2. AD DS Installation Failed on First Attempt  
**Issue:** The server returned an error during the AD DS role installation.  
**Fix:**  
- Restarted the server and retried the installation.  
- Verified that Windows updates did not have pending restarts.

### 3. Domain Promotion Taking Too Long  
**Issue:** "Promoting this server to a domain controller" froze at 50–60%.  
**Fix:**  
- Increased RAM from 2GB → 4GB temporarily.  
- Closed other heavy applications on the host system.

### 4. Cannot Create Organizational Units  
**Issue:** “Access Denied” error when creating OUs.  
**Fix:**  
- Ensured I was logged in as **Domain Administrator** (not local admin).  
- Restarted the AD Administrative Center service.

### 5. DNS Error After Domain Controller Setup  
**Issue:** Clients unable to join the domain; DNS lookup failures.  
**Fix:**  
- Verified DNS service was running in Server Manager.  
- Confirmed server’s own DNS points to **127.0.0.1** or its static IP.  
- Flushed DNS cache using:  


---

## ✅ Final Outcome
Active Directory domain created and ready for GPO and user management.
