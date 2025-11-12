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
The pfSense OS was installed via the Netgate Installer. ZFS was used as the file system, and a strong root password was set for console access.

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
Phase 0, Step 4: Configuring the Custom LAN/OPT1 IP addresses to establish the secure lab network.

