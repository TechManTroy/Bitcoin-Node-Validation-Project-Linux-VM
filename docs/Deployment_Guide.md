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

