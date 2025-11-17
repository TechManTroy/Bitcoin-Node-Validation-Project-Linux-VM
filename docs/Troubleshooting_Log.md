# Troubleshooting Log

## Issue T-01: Non-Standard Installation Media Compression

| Category | Networking/Setup |
| :--- | :--- |
| **Phase/Step** | Phase 0, Step 1 (Media Acquisition) |
| **Date** | 2025-11-11 |

### Problem (P)
The official pfSense ISO image was downloaded as a multi-layered compressed archive (.gz), which was not recognized by the Windows host OS's native file extractor. Attempts to use the compressed file resulted in a failure to mount in VirtualBox.

### Action (A)
1.  **Specialized Tool Acquisition:** Installed the open-source decompression utility **7-Zip** to handle the `.gz` file format, demonstrating reliance on specialized tools for system utility.
2.  **Multi-Step Extraction:** Used 7-Zip's **Extract Files** function to perform the two-step decompression necessary to reveal the final usable **`.iso`** image (e.g., extracting from `.gz` to an intermediate file, then to the final `.iso`).

### Result (R)
The usable `.iso` installation file was successfully acquired. This established a robust workflow for handling non-standard archives, validating the integrity of the media before proceeding with the installation.

## Issue T-02: Failure of OPT1 Management Link (Host-Only Network)

| Category | Networking/Virtualization |
| :--- | :--- |
| **Phase/Step** | Phase 0, Step 5 (Windows Host Configuration) |
| **Date** | 2025-11-16 |

### Problem (P)
The Windows Host PC was unable to establish a secure management connection to the pfSense OPT1 interface (192.168.56.2), resulting in a "Site can't be reached" error and failed ping tests. The failure persisted despite:
1.  Verifying static IP configuration on the Host-Only adapter (Host: 192.168.56.10).
2.  Disabling/enabling the adapter.
3.  Resetting the entire Windows networking stack (`netsh winsock/int ip reset`).

Investigation revealed the Host-Only adapter was categorized by Windows Defender Firewall as a "Public network," resulting in an overly strict inbound blocking policy that prevented HTTPS/ICMP traffic from the VM.

### Action (A)
1.  **Abandoned Broken Interface:** The OPT1 (Host-Only) interface was deemed permanently unreliable for management access due to persistent driver/firewall conflicts on the Host OS.
2.  **Implemented Two-Step SSH Management:** A professional workaround was adopted, utilizing the **functional LAN interface** for management. This requires SSH access to the pfSense WAN IP, followed by an SSH jump to the Node's LAN IP.
3.  **Prioritized Core Goal:** Proceeded with **Phase 0, Step 6 (Node VM Creation)**, confirming that the primary objective (Node isolation on `SECURE_LAN`) remained intact and functional.

### Result (R)
The dedicated management link (OPT1) was bypassed. Management access will now be handled via a **secure two-jump SSH route** through the working WAN interface. The project was successfully unblocked by isolating the fault and adopting a more robust, professional solution.

## Issue T-03: Missing Admin Password

| Category | Security Hardening |
| :--- | :--- |
| **Phase/Step** | Phase 0, Step 4 (Initial Console Setup) |
| **Date** | 2025-11-17 |

### Problem (P)
The pfSense administrative user (`admin`) was left with the default password (`pfsense`) or no password set after the installation and interface assignment, constituting a critical security vulnerability.

### Action (A)
Used console option **3 (Reset admin account and password)** to set a strong, unique password for the `admin` user, securing console and web GUI access before proceeding with network exposure.

### Result (R)
The primary firewall administrative credentials were successfully secured, fulfilling the security hardening requirement for the infrastructure phase.



## Issue T-04 : Network Connection

### Troubleshooting Log for Phase 0 step 5: Network & Access Failures

This log details the persistent network conflicts and system service failures encountered during infrastructure setup (Phase 0) and the initial software installation (Phase 1). The log is vital for demonstrating problem-solving ability using the **Problem-Action-Result (PAR)** framework.

| Date | Issue/Error | Root Cause (P) | Action/Workaround (A) | Status |
| :--- | :--- | :--- | :--- | :--- |
| Nov 11 | **Host-Only Adapter Failure / Site Unreachable** | The VirtualBox Host-Only Adapter was confirmed to be non-functional, blocking the dedicated management link to the firewall. | **Strategy Pivot:** Abandoned Host-Only adapter. Moved to a **Two-Jump SSH management route** (Host $\rightarrow$ pfSense WAN $\rightarrow$ Node LAN). | Resolved |
| Nov 17 | **LAN Conflict / No DHCP Lease** | The Node VM was failing to communicate its DHCP request to pfSense, leaving the system without a working network and DNS. | **Static Configuration:** Manually fixed the Node's Netplan file (`50-cloud-init.yaml`) with a **Permanent Static IP** (`10.10.10.100`), specifying the static gateway and DNS server. | Resolved |
| Nov 17 | **`ssh: Connection closed` on WAN** | pfSense's default security policy was blocking the incoming SSH connection on the WAN interface, even though the service was enabled. | **Firewall Policy Fix:** Temporarily disabled the pfSense firewall (`pfctl -d`), established the SSH connection, and then added a **permanent rule** via console to allow SSH access from the Host's WAN-side IP. | Resolved |
| Nov 17 | **Missing Network Services** | The minimal Ubuntu Server installation lacked critical components (`systemd-networkd`, `systemctl`) required to execute the static Netplan configuration. | **Software Install:** Used low-level `ip` commands to temporarily restore DNS, then installed the missing `systemd-networkd` package via `apt`. | Resolved |
| Nov 17 | **Routing Failure / Hostname Error** | The Linux kernel was losing the essential **default gateway route** needed to reach the internet after network restarts. | **Final CLI Fix:** Used **`sudo ip route add default via 10.10.10.1`** combined with permanent static entries in Netplan to enforce the route and stabilize the network. | Resolved |
| **FINAL OUTCOME** | **Instability Resolved** | Multiple low-level conflicts led to network instability. | **Final Action:** Achieved stable, persistent connectivity, allowing **successful SSH login** to the Node VM and progression to software installation. | **SUCCESS** |



