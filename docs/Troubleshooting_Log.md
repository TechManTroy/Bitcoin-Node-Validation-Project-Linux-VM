# 🛠️ Troubleshooting & Engineering Log

## Overview

This document serves as a repository for technical roadblocks encountered during the Bitcoin Node Validation Project. Each entry follows the **PAR (Problem-Action-Result)** framework to document the diagnostic process and final resolution.

---

## 🏗️ Phase 0: Infrastructure & Virtualization Issues

### T-01: Compressed Installation Media Incompatibility

* **Problem:** Official pfSense images were downloaded in `.gz` format, which Windows native tools failed to extract correctly, leading to mount errors in VirtualBox.
* **Action:** Acquired **7-Zip** for specialized decompression. Executed a multi-step extraction to reveal the final `.iso`.
* **Result:** Established a robust workflow for handling non-standard archives and validated media integrity.

### T-02: Management Link Failure (The Strategic Pivot)

* **Problem:** The dedicated **OPT1 (Host-Only)** interface was blocked by Windows Defender (categorized as "Public"), preventing GUI and Ping access. Resetting the network stack failed to resolve the driver/firewall conflict.
* **Action:** **Strategic Pivot.** Abandoned the unreliable Host-Only adapter. Implemented a **Two-Jump SSH route** (Host  pfSense WAN  Node LAN) to maintain management access.
* **Result:** Bypassed faulty Windows drivers and adopted a more professional, command-line-centric management style.

### T-03: Default Credential Vulnerability

* **Problem:** The `admin` account was left with the default `pfsense` password after initial setup, creating a critical security risk.
* **Action:** Utilized console option **3** to reset the account with a unique, high-entropy password.
* **Result:** Secured the administrative perimeter before internet exposure.

---

## 🌐 Phase 1: Networking & Connectivity Conflicts

### T-04: DHCP Lease & Netplan Conflicts

* **Problem:** The Node VM failed to receive a DHCP lease from pfSense, resulting in "No Network" status.
* **Action:** Manually edited the Ubuntu Netplan configuration (`50-cloud-init.yaml`) to assign a **Permanent Static IP** (`10.10.10.100`) and defined the gateway/DNS manually.
* **Result:** Eliminated dependency on DHCP and stabilized internal node addressing.

### T-05: WAN-Side SSH Blocking

* **Problem:** pfSense default security policy blocked incoming SSH on the WAN interface, even with the service active.
* **Action:** Temporarily bypassed the packet filter (`pfctl -d`) to gain access, then applied a **Permanent Firewall Rule** via the console to allow SSH from the Host IP.
* **Result:** Successfully established the first leg of the "Two-Jump" management route.

### T-06: Missing Systemd Network Services

* **Problem:** The minimal Ubuntu Server install lacked `systemd-networkd`, preventing the Netplan configuration from applying.
* **Action:** Used low-level `ip` commands to force a temporary internet route, then used `apt` to install the missing network packages.
* **Result:** Restored the system's ability to manage network configurations via standard Linux utilities.

### T-07: Routing & Gateway Instability

* **Problem:** The Linux kernel was losing its **Default Gateway** route after reboots, breaking internet connectivity for the node.
* **Action:** Executed **`sudo ip route add default via 10.10.10.1`** and hardcoded the route into the Netplan file to ensure persistence.
* **Result:** Achieved 100% network stability, verified by successful outbound pings and blockchain synchronization.

---

## 📈 Final Troubleshooting Outcome

Despite multiple driver, routing, and firewall conflicts, the project was successfully unblocked through a combination of **infrastructure pivoting** and **low-level CLI diagnostics**. These resolutions ensured the Bitcoin Node is fully isolated, stable, and manageable.







