# Phase 0, Step 1: Software Installation

## Overview
This step covers the download and installation of all necessary third-party software required to build the virtualized Bitcoin Node environment on the host machine.

## Installed Components

| Software | Version | Purpose | Installation Status |
| :--- | :--- | :--- | :--- |
| Oracle VM VirtualBox | Version 7.2.4 | Hypervisor for running virtual machines. | COMPLETE |
| VirtualBox Extension Pack | Version 7.2.4 | Provides enhanced features (e.g., USB 3.0, RDP). | COMPLETE |

## Downloaded Operating System Images (ISOs)

| Image File | Version | Purpose | File Location |
| :--- | :--- | :--- | :--- |
| netgate-installer | v1.1 | Firewall and Router VM operating system. | /Bitcoin-Node-Project/ISOs/netgate-installer-v1.1-RELEASE-amd64 |
| pfSsense ISO | 2.8.1 | Firewall and Router VM operating system. | /Bitcoin-Node-Project/ISOs/pfSense-CE-[version].iso |
| Ubuntu Server ISO | 24.04.3 LTS | Bitcoin Node VM operating system. | /Bitcoin-Node-Project/ISOs/ubuntu-24.04.3-live-server-amd64.iso |

## Next Step

# Phase 0, Step 2: pfSense VM Provisioning and Interface Configuration

## 2.1 Virtual Machine Provisioning
The pfSense VM was created and provisioned with the necessary resources for a virtual firewall.

| Setting | Value | Rationale |
| :--- | :--- | :--- |
| VM Name | pfSense-Router-VM | |
| Type | FreeBSD (64-bit) | Required OS type. |
| RAM | 2048 MB (2 GB) | Sufficient resources for firewall operations. |
| Processors | 1 CPU | Adequate for routing and firewall processing. |
| Storage | 16 GB VDI (Dynamically Allocated) | Adequate space for OS and firewall logs. |

## 2.2 Network Adapter Configuration and Security Hardening
Three adapters were configured to create the secure, isolated lab network. Promiscuous Mode was enabled for all adapters to ensure the firewall could inspect all necessary traffic (a requirement for its functionality).

| Adapter | VirtualBox Setting | pfSense Role | Security Setting |
| :--- | :--- | :--- | :--- |
| Adapter 1 | **Bridged Adapter** | WAN (Internet Gateway) | Promiscuous Mode: **Allow All** |
| Adapter 2 | **Internal Network (SECURE_LAN)** | **LAN (Node Network)** | Promiscuous Mode: **Allow All** |
| Adapter 3 | **Host-Only Adapter** | OPT1 (Management Access) | Promiscuous Mode: **Allow All** |

## Next Step
# Phase 0, Step 3: pfSense OS Installation and Initial IP Assignment

## 3.1 Installation Procedure (Netgate Installer)
The pfSense OS was installed via the Netgate Installer. ZFS was used as the file system.

## 3.2 Interface Assignment Confirmation
The three network interfaces were successfully mapped to their logical roles at the pfSense console prompt:

| pfSense Role | Device Name Used (CRITICAL) | Rationale |
| :--- | :--- | :--- |
| **WAN** | **em0** | Internet Gateway (Bridged Adapter) |
| **LAN** | **em1** | **SECURE_LAN** (Isolated Node Network) |
| **OPT1** | **em2** | Management Access (Host-Only Adapter) Will Configure in next step |

## 3.3 Default Network Status
The LAN interface is currently operating on the default network:

* **LAN IP Address (Default):** 192.168.1.1/24 (Will Configure in next step)

## Next Step
# Phase 0, Step 4: Custom IP Configuration and Management Access Setup (Completed)

## 4.1 Custom IP Hardening Confirmed
The necessary static IPs and services are now active on the pfSense firewall:

* **LAN IP Address:** 10.10.10.1/24 (DHCP enabled)
* **OPT1 IP Address:** 192.168.56.2/24 (Static Management IP)

## Next Step
# Phase 0, Step 5: Provisioning Bitcoin Node VM (Ubuntu Server)

## 5.1 Virtual Machine Provisioning
The Node VM was provisioned with high-performance resources, leveraging the Host's NVMe drive for optimal IBD speed.

| Setting | Value | Rationale |
| :--- | :--- | :--- |
| VM Name | Bitcoin-Node-VM | |
| RAM | 8 GB | Optimizes Initial Block Download (IBD) speed. |
| Processors | 4 CPUs | Allocates cores for verification/hashing. |
| Storage | 850 GB VDI (on NVMe SSD) | Guarantees sufficient space and high I/O performance. |

## 5.2 Network Connection (Final Secure Architecture)
The Node VM was successfully placed behind the pfSense firewall.

* **Adapter 1:** Attached to **Internal Network**
* **Network Name:** **SECURE_LAN**
* **Goal:** The Node VM will automatically receive its IP address from pfSense's DHCP server (10.10.10.100-200 range).

## 5.3 Management Workaround (Final)
The broken Host-Only adapter link was abandoned. Management will proceed via a secure **Two-Jump SSH** route:
1.  **Host PC** -> **pfSense WAN IP** (`192.168.1.x`)
2.  **pfSense Console** -> **Node LAN IP** (`10.10.10.x`)

## Next Step
Phase 1: Installing and Configuring Bitcoin Core.

