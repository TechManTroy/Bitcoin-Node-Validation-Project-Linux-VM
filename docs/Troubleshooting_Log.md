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
