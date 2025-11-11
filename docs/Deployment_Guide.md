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
| pfSsense ISO | [Record your version] | Firewall and Router VM operating system. | /Bitcoin-Node-Project/ISOs/pfSense-CE-[version].iso |
| Ubuntu Server ISO | 24.04.3 LTS | Bitcoin Node VM operating system. | /Bitcoin-Node-Project/ISOs/ubuntu-24.04.3-live-server-amd64.iso |

## Next Step

# Phase 0, Step 2: Create and Install the pfSense Router VM.

## 2.1 Virtual Machine Provisioning
The pfSense VM was created with the necessary resources and network segmentation.

| Setting | Value | Rationale |
| :--- | :--- | :--- |
| VM Name | pfSense-Router-VM | |
| RAM | 2048 MB (2 GB) | |
| Storage | 16 GB VDI | |

## 2.2 Network Adapter Configuration
Three interfaces were assigned to facilitate network segmentation and host management access.

| Adapter | VirtualBox Setting | pfSense Role | Device Name (Actual) |
| :--- | :--- | :--- | :--- |
| Adapter 1 | **Bridged Adapter** | WAN (Internet Gateway) | [Record actual name used, e.g., vtnet0] |
| Adapter 2 | **Internal Network (SECURE_LAN)** | **LAN (Node Network)** | [Record actual name used, e.g., vtnet1] |
| Adapter 3 | **Host-Only Adapter** | OPT1 (Management Access) | [Record actual name used, e.g., vtnet2] |

## 2.3 Interface Assignment Confirmation
Interfaces were successfully assigned at the console. The LAN interface is currently operating on the default network:

* **LAN IP Address (Default):** 192.168.1.1/24

## Next Step
Phase 0, Step 3: Changing the default LAN IP and accessing the Web GUI.
