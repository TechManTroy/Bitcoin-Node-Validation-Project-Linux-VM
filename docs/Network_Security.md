# 🛡️ Network Security Documentation (Updated)

## 1. Perimeter Security & NAT Redirection

Since pivoting to a single-VM architecture, the network perimeter is managed at the residential gateway (**Spectrum Router**). To allow the Bitcoin Node to communicate with the global peer-to-peer network while maintaining a low profile, **Destination NAT (Port Forwarding)** was configured with a custom obfuscation layer.

### 🔐 Custom Port Obfuscation

Instead of a standard 1:1 mapping, a custom external port was implemented. This is a strategic security move to reduce "noise" from automated bots that scan the internet specifically for the default Bitcoin port (8333).

| Setting | Value | Rationale |
| --- | --- | --- |
| **Service Name** | `Bitcoin-Node-Inbound` | Descriptive label in Spectrum Advanced Settings. |
| **External Port** | **49383** | **Obfuscated Port:** Masked entry point to deter common scanners. |
| **Internal Port** | **8333** | The standard port where the Bitcoin Core daemon listens. |
| **Protocol** | TCP | Required for Bitcoin peer-to-peer data transmission. |
| **Internal IP** | `192.168.1.x` | The static/reserved IP of the Ubuntu Node VM. |

---

## 2. Updated Infrastructure Traffic Flow

The flow of inbound traffic now follows this secure path:

1. **External Traffic** hits your Spectrum Public IP on Port **49383**.
2. **Spectrum Router** translates and forwards this to the **Ubuntu VM** on Port **8333**.
3. **Ubuntu UFW (Firewall)** allows the connection only into the **Bitcoin Core** process.

---

## 3. Remote Management (2-Jump SSH)

To ensure management traffic is never exposed to the public internet or the obfuscated Bitcoin port, the SSH path remains strictly internal.

* **Management Port:** 22 (Internal only).
* **Access Path:** Host PC ➔ SSH Tunnel ➔ Ubuntu Server.
* **Hardening:** Modified `sshd_config` via `nano` to restrict inbound SSH to specific internal management IPs.

---

## 4. Verification Proof

To confirm the Spectrum port forwarding is active, the following validation was performed:

* **Inbound Check:** `bitcoin-cli getconnectioncount` confirms inbound peers are successfully connecting via the redirected path.
<img width="1140" height="94" alt="Screenshot 2026-01-01 092407" src="https://github.com/user-attachments/assets/ce6ceeb6-63a1-4fbb-9666-6efdff88d4d4" />

* **Network Status:** `bitcoin-cli getnetworkinfo` shows the node is "Listening" on the network.
<img width="1275" height="793" alt="Screenshot 2026-01-01 095021" src="https://github.com/user-attachments/assets/26b43a9a-f8e8-441e-a15c-e8730a949e69" />

---




