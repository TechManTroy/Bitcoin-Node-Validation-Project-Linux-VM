# 🔒 Network Security Architecture and Policy

## 1. Network Topology and Design Rationale (Phase 0)

This section justifies the custom three-adapter network design, demonstrating the crucial isolation of the Node VM.

| Component | Network Interface (Adapter) | IP Range / CIDR | Security Rationale |
| :--- | :--- | :--- | :--- |
| **WAN (Internet)** | Adapter 1 (Bridged) | DHCP (from host router) | Allows the firewall to access the real internet (source of block data). |
| **LAN (Secure Node)** | Adapter 2 (Internal Network: `SECURE_LAN`) | **10.10.10.1/24** | **CRITICAL ISOLATION.** Creates a private subnet, completely invisible to the main home network, forcing all Node traffic through pfSense. |
| **MGMT (Management)** | Adapter 3 (Host-Only) | **192.168.56.2/24** | **Dedicated Control Plane.** Provides a secure, isolated channel for the host PC to manage the firewall. |

---

## 2. Firewall Rules Policy (Implemented in pfSense Web GUI)

This documents the core security stance of the firewall (Default Deny) and the explicit rules needed for the system to function.

### A. Default Blocking Behavior
* **Policy:** The firewall operates under the principle of **Default Deny** on the WAN interface. This means all unsolicited traffic originating from the WAN (the internet) destined for the LAN (the Node network) is implicitly **BLOCKED** by default.
* **Action:** **Promiscuous Mode** was enabled on all adapters to ensure the firewall correctly inspects all necessary traffic on the isolated segment.

### B. Explicit Pass Rules (Required for Functionality)
These rules are the bare minimum needed for the Node VM to function and get the blockchain.

| Interface | Protocol / Action | Source | Destination | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **LAN** | **PASS** (Any) | **LAN Net** | **Any** | **Required Egress.** Allows the Bitcoin Node to initiate connections to the WAN (Internet) to download data. |
| **WAN** | **PASS** (TCP/UDP) | **Any** | **WAN Address: Port 53** | Allows DNS resolution (if required for block synchronization). |

### C. Custom Node Contribution Rule (Future State)
* **Goal:** Once the Node is fully synced, a rule will be created on the WAN interface to allow inbound traffic on **Port 8333** (the Bitcoin P2P port), restricted only to the Node's internal IP (`10.10.10.x`), to contribute blocks to the network.

---

## 3. Security Hardening and Access Control

This confirms the steps taken to secure the administrative side of the firewall and the node.

* **pfSense Admin Access:** The default `admin`/`pfsense` credentials will be **immediately changed** during the setup wizard (Phase 0, Step 5).
* **Root Console Access:** The strong `root` password was set during the installation phase.
* **Node Access:** (Future State - Phase 3) **Root SSH login will be disabled** on the Node VM, and only secure, key-based authentication will be used.
