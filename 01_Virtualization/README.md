# 01 – Virtualization Lab

## 📌 Overview
In this lab, I set up a virtualized IT lab environment using a hypervisor.  
This environment will serve as the foundation for all future projects.

---

## 🛠️ Tools Used
- VirtualBox
- Windows ISO files

---

## 🧩 Steps Performed

### 1. Installed the Hypervisor
📸 *Screenshot: VirtualBox 7.2.4 Hypervisor Successfully Installed*
<img width="768" height="613" alt="Screenshot22124" src="https://github.com/user-attachments/assets/14a67292-8e60-4423-be2e-7e0cb52acb4b" />

---

### 2. Created the Base Virtual Machines
📸 *Screenshot: Windows 10 / Windows Server VM creation settings*
<img width="1192" height="827" alt="Screenshot025255" src="https://github.com/user-attachments/assets/489fa1e8-42c5-4d86-9887-83b3081ebd97" />
<img width="1196" height="826" alt="Screenshot 025046" src="https://github.com/user-attachments/assets/651e59dd-6a07-402a-8ff3-0ad6b98ee256" />
<img width="1194" height="821" alt="Screenshot24856" src="https://github.com/user-attachments/assets/0750430f-a794-4d56-ae2a-1f178628a73c" />
<img width="1191" height="829" alt="Screenshot 025658" src="https://github.com/user-attachments/assets/73c1cdcb-f521-4c77-9576-a322bf05bb84" />
<img width="1203" height="822" alt="Screenshot 025717" src="https://github.com/user-attachments/assets/3412d443-a1a9-44f3-bf33-d17d442b1932" />
<img width="1200" height="832" alt="Screenshot  025734" src="https://github.com/user-attachments/assets/7a27e769-38a9-47ee-8ca2-8896a4ba72d5" />
<img width="1920" height="1080" alt="Screenshot025748" src="https://github.com/user-attachments/assets/2ebd2fc7-13eb-4029-9b5b-a3c1a1c381cf" />

---

### 3. Configured Networking (NAT / Host-Only)
📸 *Screenshot: Network settings panel*
<img width="1920" height="1080" alt="Screenshot10 033434" src="https://github.com/user-attachments/assets/6d1dc7db-96c4-4e53-9b25-a5b8ad3dc97b" />
<img width="1920" height="1080" alt="Screenshot034022" src="https://github.com/user-attachments/assets/2152e739-6f50-473e-829d-57056c4d61a5" />
<img width="1920" height="1080" alt="Screenshot034110" src="https://github.com/user-attachments/assets/3c947677-28f8-46bd-b2e1-e072ba02e9a2" />


---

### 4. Installed OS on Each VM
📸 *Screenshot: OS installation progress*
<img width="1023" height="892" alt="Screenshot035912" src="https://github.com/user-attachments/assets/19f0a87a-9630-4c12-a59d-038fc7f9bd39" />
<img width="1031" height="899" alt="Screenshot40149" src="https://github.com/user-attachments/assets/9ec30ce4-e622-493e-a7bd-c969f4acf906" />
<img width="1025" height="890" alt="Screenshot 44954" src="https://github.com/user-attachments/assets/db1acd06-8229-4e65-a033-4b6d5ca05dba" />
<img width="1026" height="897" alt="Screenshot060639" src="https://github.com/user-attachments/assets/2e2469d2-be35-47f2-9b92-a1bb96fa59de" />


---

### 5. Took Initial Snapshots
📸 *Screenshot: Snapshot created*
<img width="1919" height="1079" alt="Screenshot52840" src="https://github.com/user-attachments/assets/d1001189-dd7c-4708-b2fd-d4ab1ca27ecb" />

---

## 📘 What I Learned
- How virtualization works  
- VM hardware allocation best practices  
- How to configure NAT vs Host-only networks  
- Importance of snapshots  

---

## ❗ Issues & Fixes (VirtualBox Windows VM Troubleshooting Log)

### Issue 1: VM Performance Extremely Slow During Installation
**Symptoms:**  
Windows installation moved slowly and the VM felt unresponsive.  
**Cause:**  
VM was assigned low resources (2GB RAM, 1 CPU).  
**Fix:**  
Increased allocation to 4GB RAM and 2 vCPUs under `Settings → System`, which improved performance.

### Issue 2: “No Bootable Medium Found” Error
**Symptoms:**  
Black screen displaying: FATAL: No bootable medium found.  
**Cause:**  
ISO file was not attached to the VM.  
**Fix:**  
Attached the Windows ISO under `Settings → Storage → Optical Drive` and restarted the VM.

### Issue 3: Installation Reboot Loop
**Symptoms:**  
VM kept returning to Windows setup after installation.  
**Cause:**  
Boot order prioritized the ISO instead of the virtual hard disk.  
**Fix:**  
Updated `Settings → System → Boot Order` to boot from Hard Disk first.

### Issue 4: VirtualBox Guest Additions Failed to Install
**Symptoms:**  
No full-screen mode, no better resolution, no mouse integration.  
**Cause:**  
Windows Defender blocked Guest Additions driver installation.  
**Fix:**  
Temporarily disabled Real-Time Protection, reinstalled Guest Additions, and rebooted.

### Issue 5: No Internet Connectivity in the VM
**Symptoms:**  
Windows showed “No Internet” despite network adapter being attached.  
**Cause:**  
Firewall blocked new NAT connections.  
**Fix:**  
Switched network mode to Bridged Adapter under `Settings → Network`. Internet worked immediately.

### Issue 6: Windows Activation Watermark
**Symptoms:**  
“Activate Windows” watermark appeared.  
**Cause:**  
Using Windows Evaluation edition for homelab testing.  
**Fix:**  
Confirmed evaluation mode is valid for lab use. No further action required.

### Issue 7: Snapshot Creation Took Too Long
**Symptoms:**  
Snapshot creation seemed to freeze or progress slowly.  
**Cause:**  
Snapshot was taken while the VM was still running.  
**Fix:**  
Shut down the VM and recreated the snapshot while Powered Off, which completed instantly.

### Issue 8: Corrupted ISO Download
**Symptoms:**  
Windows setup failed or displayed “This image cannot be used”.  
**Cause:**  
ISO download was incomplete or corrupted.  
**Fix:**  
Re-downloaded the ISO from the official Microsoft site and verified the file size before using it.

### Issue 9: VT-x/AMD-V Virtualization Error
**Symptoms:**  
Error message: “VT-x/AMD-V hardware acceleration is not available.”  
**Cause:**  
Virtualization was disabled in BIOS.  
**Fix:**  
Enabled Intel VT-x / AMD-V in BIOS CPU settings. VM started successfully afterward.

---

## ✅ Final Outcome
A fully functional virtual lab environment to run the remaining 12 projects.
