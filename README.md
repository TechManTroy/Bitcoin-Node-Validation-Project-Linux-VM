# 🛡️ Secure Bitcoin Full Node Deployment & Validation Project

## Overview

This repository documents the deployment of a fully isolated and hardened Bitcoin Full Node (Bitcoin Core) built entirely within a virtualized environment.

Originally designed with a multi-tiered pfSense architecture, the project evolved through a **strategic pivot** into a high-performance, single-VM production model. This transition highlights a "Security-in-Depth" approach, moving the defensive perimeter from the network layer to the OS and ISP levels.

---

## 🎯 Key Project Goals & Technical Achievements

| Goal | Skill Demonstrated | Achievement |
| --- | --- | --- |
| **Infrastructure Pivot** | **Strategic Problem Solving** | Diagnosed hypervisor driver instability and **successfully pivoted architecture** to a stable, single-VM model without compromising node isolation. |
| **Network Security** | **NAT & Port Obfuscation** | Implemented custom port forwarding (**Port 49383  8333**) on the Spectrum gateway to hide the service from automated global scanners. |
| **System Hardening** | **Linux System Administration** | Deployed a minimal Ubuntu Server OS on NVMe storage. Secured the environment via **UFW (Uncomplicated Firewall)** and SSH configuration hardening. |
| **Connectivity Mastery** | **Routing & Persistence** | Resolved routing deadlocks by manually configuring **Static IPs** and persistent **Default Gateway routes** within the Linux kernel via Netplan. |
| **Management Resiliency** | **Secure Remote Access** | Established a secure **Two-Jump SSH management link**, ensuring all administrative traffic is encrypted and isolated from the public Bitcoin protocol traffic. |

---

## ⚙️ Final Architecture & Configuration Summary

### 1. Virtual Network Topology

The project operates on a hardened virtual stack designed for maximum uptime and security:

| Component | Network Method | Configuration / Role |
| --- | --- | --- |
| **Bitcoin Node VM** | Bridged Adapter | **`10.10.10.100`** (Static Internal IP) |
| **Inbound Path** | Spectrum NAT Redirection | **External 49383  Internal 8333** (Obfuscated P2P) |
| **Management Link** | Secure Two-Jump SSH | Encrypted CLI management from Host PC to Node. |

### 2. Software & Services

* **Hypervisor:** VirtualBox 7.2.4
* **Node OS:** Ubuntu Server 24.04.3 LTS
* **Core Service:** Bitcoin Core (`bitcoind`)
* **Security Layer:** UFW (Linux Firewall) + Spectrum Perimeter NAT

---

## 🏗️ The Engineering Pivot (Lessons Learned)

A hallmark of this project was the transition from a pfSense-layered network to the current model.

* **Challenge:** Persistent VirtualBox Host-Only driver corruption prevented stable management access.
* **Solution:** Simplified the network architecture to eliminate driver-level points of failure while shifting security policies to **UFW** and **Spectrum Port Forwarding**.
* **Result:** Achieved 100% blockchain synchronization stability and reduced system overhead.

---

## 🚀 Next Steps (Security Hardening - Phase 3)

The next phase focuses on advanced user access controls:

1. **SSH Key Authentication:** Transitioning to RSA/Ed25519 keys and **disabling password-based login**.
2. **Automated Defense:** Implementing `fail2ban` to automatically blacklist IPs attempting to brute-force the open management or P2P ports.

---

### ➡️ Repository Contents

* **`/docs`**:
* `Deployment_Guide.md`: Sequential build log and installation steps.
* `Network_Security.md`: Port obfuscation policy and firewall rules.
* `Troubleshooting_Log.md`: Detailed "Problem-Action-Result" (PAR) history of the architecture pivot.


