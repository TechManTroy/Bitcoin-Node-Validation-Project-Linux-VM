# 📑 Bitcoin Node Validation Project: Final Deployment Guide

## 🏁 Phase 0: Infrastructure & Virtualization

### Step 1: Software & Environment Setup

This step covers the base hypervisor and OS images required for the project.

| Component | Version | Purpose |
| --- | --- | --- |
| **Oracle VM VirtualBox** | 7.2.4 | Hypervisor for running the node environment. |
| **Ubuntu Server ISO** | 24.04.3 LTS | The Operating System for the Bitcoin Node. |

---

### Step 2: The Project Pivot (Lessons Learned) 🔄

*Initially, the architecture included a pfSense virtual firewall with a complex 3-adapter network (WAN, SECURE_LAN, OPT1). This method was abandoned due to persistent Host-Adapter driver corruption and management roadblocks.*

**The Decision:** To ensure a stable and production-ready environment, the networking was simplified. The Bitcoin Node now resides within a single, hardened Ubuntu VM using a **Bridged Adapter** for direct, reliable communication while maintaining security at the OS level.

---

### Step 3: Provisioning the Bitcoin Node VM

The VM was provisioned with high-performance resources to handle the **Initial Block Download (IBD)** efficiently.

| Setting | Value | Rationale |
| --- | --- | --- |
| **VM Name** | `Bitcoin-Node-Prod` | Dedicated production environment name. |
| **RAM** | 8 GB | Optimizes block verification and hashing speeds. |
| **CPU** | 4 Cores | Allocates sufficient threads for multi-threaded verification. |
| **Storage** | 850 GB VDI | Located on Host NVMe for high I/O performance. |
| **Network** | Bridged Adapter | Ensures stable internet connectivity and remote access. |

---

### Step 4: OS Hardening & Troubleshooting

Before installing the node software, the OS was stabilized.

* **Driver Fix:** Reinstalled VirtualBox Guest Additions to resolve corrupted adapter drivers.
* **Connectivity:** Verified internet path by successfully pinging `8.8.8.8`.
* **Remote Access:** Configured a **2-Jump SSH connection** for secure management from the Host PC to the server environment.

---

## ⚡ Phase 1: Bitcoin Core Installation & Validation

### Step 1: Installing Bitcoin Core

The node software was installed using the official Bitcoin PPA to ensure a verified and stable binary.

```bash
sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt update
sudo apt install bitcoind -y

```

### Step 2: System Validation Commands

These commands were used to verify the node is successfully integrated into the global Bitcoin infrastructure.

| Goal | Command |
| --- | --- |
| **Check Connections** | `bitcoin-cli getconnectioncount` |
| **Verify Sync Status** | `bitcoin-cli getblockchaininfo` |
| **Network Summary** | `bitcoin-cli getnetworkinfo` |

---

### 📝 Final Documentation Status

* **Deployment Guide:** Updated to reflect the pivot from pfSense to a single-VM hardened setup. ✅



