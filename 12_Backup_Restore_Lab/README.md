# 12 – Backup & Restore Lab

## 📌 Overview

- Performed full backup and recovery testing across OS, files, and virtual machine layers
- Validated system resilience using restore points, file backups, and VM snapshots
- Confirmed successful recovery after intentional data loss

---

## 🛠️ Tools Used

- Windows Backup
- Windows System Restore
- File History
- Oracle VirtualBox (Hypervisor Snapshots)

---

## 🧩 Steps Performed

### 1. Created System Restore Point

- Opened System Properties
- Enabled system protection for OS drive
- Created a manual restore point before making system changes
- Verified restore point creation

📸 *Screenshot: Restore point created successfully*

<img width="1146" height="1079" alt="image" src="https://github.com/user-attachments/assets/d466c055-f05d-45b6-84df-813f0a1a1d79" />

---

### 2. Backed Up Important Files

- Enabled File History
- Selected target backup drive/location
- Ran manual backup of user documents
- Confirmed files were successfully backed up
  
📸 *Screenshot: File backup results*

<img width="1152" height="1079" alt="image" src="https://github.com/user-attachments/assets/909aabaf-bdcc-4e60-ae62-366477049943" />

---

### 3. Took VM Snapshot

- Powered off Windows 10 virtual machine
- Created snapshot named `Lab12_PreRestore_Snapshot`
- Snapshot captured OS, disk state, and VM configuration
- Confirmed snapshot appeared in snapshot tree
  
📸 *Screenshot: VM snapshot screen*

<img width="1263" height="1079" alt="image" src="https://github.com/user-attachments/assets/795ed0fa-ee5d-4655-8e03-c22d267a517b" />

---

### 4. Performed a Restore

- Deleted a test file inside the virtual machine
- Powered off the VM
- Restored VM to `Lab12_PreRestore_Snapshot`
- VirtualBox rollback completed successfully
- 
📸 *Screenshot: Snapshot restore process*

<img width="1279" height="1079" alt="image" src="https://github.com/user-attachments/assets/5f33549a-3333-4792-a47f-d80a10196a75" />
<img width="1146" height="1079" alt="image" src="https://github.com/user-attachments/assets/e256af73-df0f-453a-b09b-a59256fffacd" />
<img width="1153" height="1079" alt="image" src="https://github.com/user-attachments/assets/f6eb01ae-50eb-4bb6-a7e5-67c49ace6388" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/72b25079-dd35-48db-a4a5-9caa3b670dce" />

---

### 5. Validated Recovery

- Booted restored virtual machine
- Verified deleted file was recovered
- Confirmed Windows booted without errors
- System state matched pre-restore condition
  
📸 *Screenshot: final restored system*

<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/62c8a365-5a4c-4696-bb6a-0cefc36bb42f" />
<img width="1152" height="1079" alt="image" src="https://github.com/user-attachments/assets/4693d029-bc2a-4bc7-a9db-a676e4c63454" />

---

## 📘 What I Learned

- Backups must be tested, not just created
- System Restore protects OS configuration but not personal files
- File History enables granular file-level recovery
- Hypervisor snapshots provide fast and reliable full-system rollback
- VirtualBox confirms restore success through snapshot state, not pop-up messages 

---

## ❗ Issues & Fixes

- Restore button initially appeared disabled
-- Cause: VM had not diverged from snapshot state
-- Fix: Booted VM, made changes, powered off, then restored snapshot

- No confirmation message after restore
-- Cause: VirtualBox does not display success notifications
-- Fix: Verified restore via snapshot tree and recovered files

---

## ✅ Final Outcome

- System restore point created successfully
- Important files backed up and recovered
- Virtual machine snapshot restored successfully
- Deleted file fully recovered
- Windows OS booted successfully after restoration
- Lab objectives completed and validated
