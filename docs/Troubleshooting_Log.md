# 🛠️ Troubleshooting & Engineering Log

## 🔄 The Major Project Pivot: Architecture Evolution

Initially, the project was designed as a multi-VM tiered network using **pfSense** as a virtual gateway. However, due to persistent hypervisor driver conflicts and Windows Host-Only adapter instability, a strategic decision was made to pivot.

**The Pivot:** Transitioned from a pfSense-dependent network to a **Hardened Isolated Ubuntu VM**.

* **Reasoning:** To eliminate the single point of failure (corrupted VirtualBox drivers) while maintaining the core objective of a secure, dedicated Bitcoin validator.
* **Security Trade-off:** Security was moved from the *Network Perimeter* (pfSense) to the *Host/OS Level* (UFW, SSH Hardening, and Spectrum NAT rules).

---

## 🏗️ Phase 0: Infrastructure & Media Acquisition

### T-01: Compressed Installation Media Incompatibility

* **Problem (P):** The official pfSense ISO image was downloaded as a `.gz` archive, which was not recognized by the Windows native extractor, causing VirtualBox mount failures.
* **Action (A):** Installed **7-Zip** to handle multi-layered decompression, extracting the usable `.iso`.
* **Result (R):** Established a workflow for handling non-standard archives and validated media integrity.

### T-02: Failure of OPT1 Management Link (The Pivot Point)

* **Problem (P):** The Windows Host was unable to reach the pfSense management interface (`192.168.56.2`). Investigation revealed the Host-Only adapter was categorized as "Public" by Windows Defender, blocking all traffic despite stack resets.
* **Action (A):** **Strategic Pivot.** Deemed the pfSense/Host-Only link permanently unreliable. Abandoned the layered VM approach in favor of a single-VM setup with a **Bridged Adapter** and a **Two-Jump SSH management route**.
* **Result (R):** Project unblocked. Shifted focus to hardening the Ubuntu Node VM directly.

---

## 🌐 Phase 1: Networking & Connectivity Conflicts (Post-Pivot)

### T-03: Missing Admin Password

* **Problem (P):** The initial pfSense administrative user was left with default credentials, posing a risk during the testing phase.
* **Action (A):** Used console option **3** to reset and secure the admin password.
* **Result (R):** Secured the gateway credentials before shifting to the final VM architecture.

### T-04: Network Configuration & Gateway Instability

* **Problem (P):** Post-pivot, the Ubuntu VM faced multiple network hurdles: lack of DHCP lease, missing `systemd-networkd` packages, and a disappearing default gateway route.
* **Action (A):** 1. Manually configured a **Static IP** (`10.10.10.100`) via Netplan.
2. Forced a temporary route using `ip route add default via 10.10.10.1`.
3. Installed missing networking utilities via `apt`.
* **Result (R):** Achieved 100% network stability and persistent internet connectivity for the node.

### T-05: ISP-Level Perimeter Security (Spectrum Custom Port)

* **Problem (P):** Standard Bitcoin traffic (Port 8333) was being blocked at the residential gateway level.
* **Action (A):** Configured a custom port forwarding rule on the **Spectrum account** (External Port **49383**  Internal Port **8333**).
* **Result (R):** Successfully enabled inbound P2P communication while using port obfuscation to deter automated scanners.

---

## 📈 Final Engineering Outcome

The project successfully moved from a failed multi-layered simulation to a **stable, hardened, and high-performance production VM**. By troubleshooting low-level Linux routing and ISP-level port forwarding, the Bitcoin Node is now fully validated and integrated into the global network.
