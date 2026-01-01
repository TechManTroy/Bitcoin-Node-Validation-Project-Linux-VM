# ⚰️ Node Decommissioning Report: Final State & Shutdown

## 1. Executive Summary

After successful deployment, synchronization, and validation, the Bitcoin Full Node project is being officially decommissioned. This report captures the final healthy state of the node to prove its operational integrity before the virtual machine is retired and the storage is wiped.

---

## 2. Final Operational Metrics (Audit Trail)

The following metrics were captured on **January 1, 2026**, confirming a 100% synchronized and healthy node.

### ⛓️ Blockchain Synchronization

* **Chain Status:** `main` (Mainnet)
* **Total Blocks/Headers:** **930,432**
* **Verification Progress:** **99.999%** (Fully Synchronized)
* **IBD Status:** `initialblockdownload: false` (Complete)
* **Total Ledger Size on Disk:** **~809 GB**

### 🌐 Network & Connectivity

* **Local IPv6 Addresses:** Node was active and reachable on the global IPv6 network via port **8333**.
* **Active Peer Handshakes:** Live logs confirm continuous incoming/outgoing inventory (`inv`) and data requests (`getdata`) from peers like **peer=555**.
* **Connection Stability:** Verified **"outbound-full-relay"** connection types utilizing the **v2 transport protocol**.

---

## 3. Final Resource Utilization (Btop Analysis)

Final system monitoring via `btop` confirms the VM was operating within optimal parameters at the time of shutdown.

* **CPU Load:** Negligible at **~0-1%**, indicating efficient background validation.
* **Memory Footprint:** The `bitcoind` process was utilizing **1.7 GB** of the allocated 15.6 GiB RAM.
* **Storage Distribution:** Root partition (`/`) shows **880G** utilized on the high-performance NVMe virtual disk.
* **Uptime:** The system had been running stably for **21 days, 12 hours** prior to this final audit.

---

## 4. Decommissioning Checklist

The following steps were taken to securely retire the node:

1. **Graceful Shutdown:** Executed `bitcoin-cli stop` to ensure the LevelDB database was not corrupted during closure.
2. **Service Disabling:** Disabled the `bitcoind` systemd service to prevent auto-start.
3. **Firewall Cleanup:** Revoked the Spectrum custom port forwarding rule (**49383**) and closed Port 8333 on the host.
4. **Data Deletion:** The 850GB VDI file will be securely deleted from the Host NVMe to recover storage.

---

## 🏁 Final Conclusion

The project successfully met all goals of **Phase 0, 1, and 2**. It proved that a Bitcoin node could be safely isolated and managed within a virtual environment even after shifting architectural strategies.

**Documentation is now complete.**

---

### 📝 Final Documentation Status

* **Decommissioning Guide:** Final state captured and archived. ✅
* **Project Status:** **CLOSED.**
