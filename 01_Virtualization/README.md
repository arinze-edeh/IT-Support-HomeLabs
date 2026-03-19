# Lab 01 -- Virtualization Infrastructure Setup

> **Enterprise IT Lab Series** | Project 1 of 13 | Status: **Complete**

---

## Table of Contents

- [Overview](#overview)
- [Architecture Summary](#architecture-summary)
- [Prerequisites](#prerequisites)
- [Tools and Technologies](#tools-and-technologies)
- [Setup Procedure](#setup-procedure)
  - [Step 1 -- Install the Hypervisor](#step-1----install-the-hypervisor)
  - [Step 2 -- Create Base Virtual Machines](#step-2----create-base-virtual-machines)
  - [Step 3 -- Configure Networking](#step-3----configure-networking)
  - [Step 4 -- Install Operating Systems](#step-4----install-operating-systems)
  - [Step 5 -- Create Initial Snapshots](#step-5----create-initial-snapshots)
- [Troubleshooting Log](#troubleshooting-log)
- [Key Learnings](#key-learnings)
- [Outcome](#outcome)

---

## Overview

This project establishes a fully isolated, virtualized IT lab environment using **VirtualBox** as the Type 2 hypervisor. The lab provisions both a **Windows 10** client machine and a **Windows Server** instance, configured with appropriate networking modes and preserved via snapshots.

This environment serves as the **foundational infrastructure** for all subsequent lab projects in this series, enabling hands-on practice with Active Directory, networking, security, and systems administration without risk to production systems.

---

## Architecture Summary

```
Host Machine
└── VirtualBox Hypervisor
    ├── Windows 10 VM
    │   ├── Network: NAT / Host-Only Adapter
    │   ├── RAM: 4 GB | vCPUs: 2
    │   └── Snapshot: Clean Baseline
    └── Windows Server VM
        ├── Network: NAT / Host-Only Adapter
        ├── RAM: 4 GB | vCPUs: 2
        └── Snapshot: Clean Baseline
```

---

## Prerequisites

| Requirement | Minimum Spec |
|---|---|
| Host RAM | 8 GB (16 GB recommended) |
| Host Storage | 100 GB free disk space |
| CPU | Intel VT-x or AMD-V enabled in BIOS |
| OS | Windows 10/11 or Linux host |
| VirtualBox | Version 7.x or later |
| Windows ISO | Windows 10 + Windows Server (Evaluation editions accepted) |

> **Critical:** Virtualization must be enabled in BIOS before proceeding. See [Issue 9](#issue-9----vt-xamd-v-virtualization-not-available) in the troubleshooting log.

---

## Tools and Technologies

| Tool | Purpose |
|---|---|
| **VirtualBox 7.2.4** | Type 2 hypervisor for hosting virtual machines |
| **Windows 10 ISO** | Client operating system for workstation VM |
| **Windows Server ISO** | Server operating system for domain/lab services |
| **VirtualBox Guest Additions** | Enhanced VM integration (display, clipboard, mouse) |

---

## Setup Procedure

### Step 1 -- Install the Hypervisor

Download and install **VirtualBox 7.2.4** from the [official VirtualBox website](https://www.virtualbox.org/).

During installation, accept the network interface prompt. This installs the VirtualBox Host-Only Ethernet Adapter required for internal VM networking.

**Verify the installation:**
- Launch VirtualBox Manager
- Confirm version number is displayed in the title bar
- Ensure no errors appear on startup

> **Screenshot**

<img width="768" height="613" alt="Screenshot22124" src="https://github.com/user-attachments/assets/14a67292-8e60-4423-be2e-7e0cb52acb4b" />

> *Caption: VirtualBox 7.2.4 Manager successfully launched on host machine*

---

### Step 2 -- Create Base Virtual Machines

Create two separate virtual machines: one for **Windows 10** (client) and one for **Windows Server**.

**For each VM, apply the following resource configuration:**

1. Open VirtualBox Manager and select **New**
2. Set the VM name, type, and version to match the target OS
3. Allocate **4 GB RAM** and **2 vCPUs** under the System settings
4. Create a new **Virtual Hard Disk** (recommended: 50 GB, dynamically allocated)
5. Apply settings and repeat for the second VM

> **Screenshots**

<img width="1192" height="827" alt="Screenshot025255" src="https://github.com/user-attachments/assets/489fa1e8-42c5-4d86-9887-83b3081ebd97" />
<img width="1196" height="826" alt="Screenshot 025046" src="https://github.com/user-attachments/assets/651e59dd-6a07-402a-8ff3-0ad6b98ee256" />
<img width="1194" height="821" alt="Screenshot24856" src="https://github.com/user-attachments/assets/0750430f-a794-4d56-ae2a-1f178628a73c" />
<img width="1191" height="829" alt="Screenshot 025658" src="https://github.com/user-attachments/assets/73c1cdcb-f521-4c77-9576-a322bf05bb84" />
<img width="1203" height="822" alt="Screenshot 025717" src="https://github.com/user-attachments/assets/3412d443-a1a9-44f3-bf33-d17d442b1932" />
<img width="1200" height="832" alt="Screenshot  025734" src="https://github.com/user-attachments/assets/7a27e769-38a9-47ee-8ca2-8896a4ba72d5" />
<img width="1920" height="1080" alt="Screenshot025748" src="https://github.com/user-attachments/assets/2ebd2fc7-13eb-4029-9b5b-a3c1a1c381cf" />

---

### Step 3 -- Configure Networking

Each VM requires two network adapters:

| Adapter | Mode | Purpose |
|---|---|---|
| Adapter 1 | NAT | Internet access from the VM |
| Adapter 2 | Host-Only | Internal communication between VMs and host |

**Configuration steps:**

1. Select a VM and open **Settings > Network**
2. Set **Adapter 1** to **NAT**
3. Enable **Adapter 2** and set it to **Host-Only Adapter**
4. Select the appropriate host-only network from the dropdown
5. Repeat for both VMs

> **Screenshots**
<img width="1920" height="1080" alt="Screenshot10 033434" src="https://github.com/user-attachments/assets/6d1dc7db-96c4-4e53-9b25-a5b8ad3dc97b" />
<img width="1920" height="1080" alt="Screenshot034022" src="https://github.com/user-attachments/assets/2152e739-6f50-473e-829d-57056c4d61a5" />
<img width="1920" height="1080" alt="Screenshot034110" src="https://github.com/user-attachments/assets/3c947677-28f8-46bd-b2e1-e072ba02e9a2" />

> **Note:** If internet connectivity fails after OS installation, refer to [Issue 5](#issue-5----no-internet-connectivity-inside-vm) in the troubleshooting log.

---

### Step 4 -- Install Operating Systems

Attach the ISO and perform a fresh OS installation on each VM.

**Pre-installation:**

1. Open **Settings > Storage** for the VM
2. Click the optical drive and select **Choose a Disk File**
3. Browse to and select the appropriate Windows ISO
4. Confirm the ISO is mounted before starting the VM

**Installation process:**

1. Start the VM -- it should boot from the ISO automatically
2. Follow the Windows setup wizard
3. Select **Custom Install** and target the virtual hard disk
4. Allow the installation to complete and reboot

> **Screenshot**

<img width="1023" height="892" alt="Screenshot035912" src="https://github.com/user-attachments/assets/19f0a87a-9630-4c12-a59d-038fc7f9bd39" />
<img width="1031" height="899" alt="Screenshot40149" src="https://github.com/user-attachments/assets/9ec30ce4-e622-493e-a7bd-c969f4acf906" />
<img width="1025" height="890" alt="Screenshot 44954" src="https://github.com/user-attachments/assets/db1acd06-8229-4e65-a033-4b6d5ca05dba" />
<img width="1026" height="897" alt="Screenshot060639" src="https://github.com/user-attachments/assets/2e2469d2-be35-47f2-9b92-a1bb96fa59de" />

**Post-installation (recommended):**

- Install **VirtualBox Guest Additions** via **Devices > Insert Guest Additions CD**
- Enable full-screen mode and bidirectional clipboard
- Run Windows Update to apply all patches

> **Note:** If Guest Additions fails to install, see [Issue 4](#issue-4----virtualbox-guest-additions-fails-to-install) in the troubleshooting log.

---

### Step 5 -- Create Initial Snapshots

Snapshots preserve the clean post-install state of each VM. This is a critical step before any lab work begins.

**Best practice:** Always power off the VM before taking a snapshot.

1. Shut down the VM completely (not just sleep/save state)
2. In VirtualBox Manager, select the VM
3. Open the **Snapshots** panel via the dropdown next to the VM name
4. Click **Take** and name the snapshot descriptively
   - Example: `Windows10-Clean-Baseline` | `WinServer-Clean-Baseline`
5. Add a description noting the date and current state

> **Screenshot**

<img width="1919" height="1079" alt="Screenshot52840" src="https://github.com/user-attachments/assets/d1001189-dd7c-4708-b2fd-d4ab1ca27ecb" />

> *Caption: Snapshot successfully created while VM is in Powered Off state*

> **Warning:** Taking snapshots while the VM is running significantly increases completion time and may cause apparent freezing. See [Issue 7](#issue-7----snapshot-creation-is-slow-or-appears-frozen) in the troubleshooting log.

---

## Troubleshooting Log

This section documents all issues encountered during lab setup, their root causes, and verified resolutions.

---

### Issue 1 -- VM Performance Extremely Slow During Installation

**Symptoms**

- Windows installation progressed very slowly
- VM was unresponsive to mouse and keyboard input
- High CPU usage on the host machine

**Root Cause**

The VM was initially configured with insufficient resources: 2 GB RAM and 1 vCPU, which is below the minimum threshold for running a Windows installer.

**Resolution**

Power off the VM, then navigate to **Settings > System**:

- Increase RAM allocation to **4 GB**
- Increase CPU count to **2 vCPUs**
- Ensure the host machine has sufficient free resources before increasing allocation

**Status:** `Resolved`

---

### Issue 2 -- "No Bootable Medium Found" Error on Startup

**Symptoms**

- Black screen on VM boot
- Error message: `FATAL: No bootable medium found! System halted.`

**Root Cause**

The Windows ISO was not attached to the VM's virtual optical drive prior to first boot.

**Resolution**

1. With the VM powered off, navigate to **Settings > Storage**
2. Click the optical drive (CD icon) under the Storage Tree
3. Select **Choose a Disk File** and attach the Windows ISO
4. Start the VM -- it should now boot from the ISO

**Status:** `Resolved`

---

### Issue 3 -- VM Loops Back to Windows Setup After Installation

**Symptoms**

- After installation completed, VM rebooted back into the Windows setup wizard
- Installation appeared to repeat in a loop

**Root Cause**

The VM boot order prioritized the optical drive (ISO) over the virtual hard disk, causing the system to boot from the installer instead of the newly installed OS.

**Resolution**

1. Power off the VM
2. Navigate to **Settings > System > Boot Order**
3. Move **Hard Disk** to the top of the boot order list
4. Alternatively, eject the ISO from the optical drive after installation completes

**Status:** `Resolved`

---

### Issue 4 -- VirtualBox Guest Additions Fails to Install

**Symptoms**

- Guest Additions installer silently failed or terminated early
- No full-screen mode, no mouse integration, low display resolution persisted
- Windows Defender notification appeared during installation attempt

**Root Cause**

Windows Defender Real-Time Protection blocked the unsigned kernel driver components included in the Guest Additions installer package.

**Resolution**

1. Open **Windows Security > Virus and Threat Protection**
2. Temporarily disable **Real-Time Protection**
3. Re-run Guest Additions installer via **Devices > Insert Guest Additions CD Image**
4. Reboot the VM after installation completes
5. Re-enable Real-Time Protection after reboot

**Status:** `Resolved`

---

### Issue 5 -- No Internet Connectivity Inside VM

**Symptoms**

- Windows network indicator showed "No Internet" or limited connectivity
- Browser requests timed out
- Network adapter was confirmed as attached in VM settings

**Root Cause**

A host-side firewall rule was blocking outbound NAT connections originating from VirtualBox. The NAT engine was unable to forward traffic on behalf of the VM.

**Resolution**

As a quick fix, switch the network adapter to **Bridged Adapter** mode:

1. Power off the VM
2. Navigate to **Settings > Network > Adapter 1**
3. Change **Attached to** from **NAT** to **Bridged Adapter**
4. Select the active host network interface from the dropdown
5. Start the VM -- internet access should be restored immediately

> **Note:** Bridged mode exposes the VM directly on the local network and assigns it an IP from the router. For isolated lab environments, resolve the firewall rule conflict and return to NAT mode.

**Status:** `Resolved`

---

### Issue 6 -- Windows Activation Watermark on Desktop

**Symptoms**

- Persistent watermark in the bottom-right corner: `Activate Windows. Go to Settings to activate Windows.`

**Root Cause**

Windows Evaluation or unactivated edition was used, which is standard for home lab and test environments.

**Resolution**

No action required. Windows Evaluation editions are fully functional for lab testing. Activation is not necessary for the purpose of this project series.

> **Note:** If activation is required for domain-join scenarios in future labs, use a valid license key or extend the evaluation period via `slmgr /rearm` in an elevated command prompt.

**Status:** `Accepted -- By Design`

---

### Issue 7 -- Snapshot Creation Is Slow or Appears Frozen

**Symptoms**

- Snapshot progress bar stalled or did not appear
- Process took several minutes with no visible activity
- VirtualBox Manager became temporarily unresponsive

**Root Cause**

Snapshots taken while the VM is in a **running state** require VirtualBox to write a memory dump in addition to the disk state. This significantly increases the time and I/O load required.

**Resolution**

1. Shut down the VM completely before taking snapshots
2. Confirm the VM shows **Powered Off** status in VirtualBox Manager
3. Proceed with snapshot creation -- the operation will complete in seconds

**Status:** `Resolved`

---

### Issue 8 -- Windows Setup Fails or Shows "This Image Cannot Be Used"

**Symptoms**

- Windows installer crashed at the beginning of setup
- Error message: `Windows cannot be installed to this disk`
- Setup displayed "This image cannot be used" or similar corruption notice

**Root Cause**

The ISO file download was incomplete or corrupted, resulting in a checksum mismatch between the downloaded file and the official release.

**Resolution**

1. Delete the existing ISO file
2. Re-download from the official [Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/)
3. Verify the file size matches the value published on the download page before mounting

**Status:** `Resolved`

---

### Issue 9 -- VT-x/AMD-V Virtualization Not Available

**Symptoms**

- VirtualBox displayed an error on VM start: `VT-x/AMD-V hardware acceleration is not available on your system`
- No 64-bit OS options appeared in VM creation wizard

**Root Cause**

Hardware-assisted virtualization (Intel VT-x or AMD-V) was disabled in the system BIOS/UEFI firmware. This setting is off by default on many consumer motherboards.

**Resolution**

1. Restart the host machine and enter BIOS/UEFI setup (typically **F2**, **Del**, or **F10** during POST)
2. Locate the CPU configuration section
3. Enable **Intel Virtualization Technology (VT-x)** or **AMD-V / SVM Mode**
4. Save and exit BIOS
5. Reboot into the host OS and attempt to start the VM again

> **Note:** On Windows 11 hosts, also verify that **Hyper-V** is disabled if VirtualBox still reports this error after enabling virtualization in BIOS. Hyper-V and VirtualBox cannot run simultaneously without additional configuration.

**Status:** `Resolved`

---

## Key Learnings

| Concept | Takeaway |
|---|---|
| **Virtualization fundamentals** | Type 2 hypervisors run on top of a host OS and share hardware resources among guest VMs |
| **Resource allocation** | Underprovisioning RAM and CPU causes severe performance degradation during OS installation |
| **NAT vs Host-Only networking** | NAT provides internet access via the host; Host-Only creates an isolated internal network between VMs |
| **Boot order management** | Incorrect boot order is the most common cause of post-install reboot loops |
| **Snapshots** | Powered-off snapshots are instantaneous and reliable; running snapshots are slow and resource-intensive |
| **Guest Additions** | Essential for usability -- resolves resolution, clipboard, and mouse integration issues |
| **BIOS virtualization** | Must be explicitly enabled; not enabled by default on most consumer hardware |

---

## Outcome

A fully operational, dual-VM virtualized lab environment was successfully provisioned and validated. Both the **Windows 10** client VM and the **Windows Server** VM are configured with appropriate networking, Guest Additions installed, and protected by clean baseline snapshots.

This environment is ready to support the remaining **12 projects** in the series, including Active Directory, DNS, Group Policy, network security, and systems monitoring labs.

---

*Part of the **Enterprise IT Homelab Series** | Lab 01 of 13*

