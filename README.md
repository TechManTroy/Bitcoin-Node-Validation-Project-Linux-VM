# Bitcoin-Node-Validation-Project-Linux-VM

# 🛡️ Secure Bitcoin Full Node Deployment & Validation Project

## Overview
This repository documents the deployment of a fully isolated and secure Bitcoin Full Node (Bitcoin Core) inside a custom **Virtual Local Area Network (VLAN) architecture** built entirely within VirtualBox.

The project demonstrates advanced skills in **network segmentation, system hardening, and service persistence** by forcing the Node VM to operate behind a dedicated virtual firewall (pfSense), mitigating external and internal security threats.

---

## 🎯 Key Project Goals & Technical Achievements

| Goal | Skill Demonstrated | Achievement |
| :--- | :--- | :--- |
| **Network Security** | **Advanced Firewalling & Isolation** | Successfully implemented a **Two-Interface (WAN/LAN) pfSense firewall**, enforcing a **Default Deny** security posture on the Node VM's traffic. |
| **System Hardening** | **Linux System Administration (CLI)** | Deployed the Node VM using a minimal Ubuntu Server OS, reducing the attack surface. Managed the entire server via secure, persistent SSH. |
| **Network Persistence** | **Netplan & Routing Mastery** | Resolved complex routing deadlocks by implementing a **permanent static IP configuration** and adding the crucial **Default Gateway route** directly to the Linux kernel. |
| **Management Resiliency** | **Troubleshooting & Security Policy** | Bypassed initial network deadlocks to establish a functional, secure **Two-Jump SSH management link** (Host → pfSense → Node) by adding an explicit WAN firewall rule. |
| **Core Function** | **Cryptocurrency Service Management** | Installed and configured the **Bitcoin Core** client on high-performance NVMe storage to begin the Initial Block Download (IBD) and validate the entire ledger. |

---

## ⚙️ Final Architecture & Configuration Summary

### 1. Virtual Network Topology
The project operates on a custom virtual internal network to ensure isolation:

| Component | Network Method | IP Address / Role |
| :--- | :--- | :--- |
| **Firewall (pfSense)** | Bridged / Internal Network | **`10.10.10.1`** (LAN Gateway) |
| **Bitcoin Node VM** | Internal Network (`SECURE_LAN`) | **`10.10.10.100`** (Static IP) |
| **Management Link** | Secure Two-Jump SSH (Host $\rightarrow$ pfSense WAN $\rightarrow$ Node) | Full management is done via encrypted SSH from the host. |

### 2. Software & Services
* **Host OS:** Windows 11
* **Hypervisor:** VirtualBox
* **Firewall OS:** pfSense Community Edition
* **Node OS:** Ubuntu Server LTS
* **Core Service:** Bitcoin Core (`bitcoind`)

---

## 🚀 Next Steps (Security Hardening - Phase 3)
The next phase of this project will focus entirely on locking down user access to the Node VM:

1.  **SSH Key Authentication:** Implement key-based SSH and **disable password login**.
2.  **User Access Control:** Disable direct root login and use `sudo` only.

***

### ➡️ Repository Contents
* **`/docs`**: Contains the `Deployment_Guide.md` (the sequential build log), `Network_Security.md` (firewall policy and architecture justification), and `Troubleshooting_Log.md` (detailed failure and fix history).
* **`/config`**: Templates for the `bitcoin.conf` and `systemd` service files.
* **`/screenshots`**: Visual evidence of critical configurations and successful connection.


