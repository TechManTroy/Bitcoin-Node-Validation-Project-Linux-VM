# Bitcoin-Node-Validation-Project-Linux-VM

## File Structure

Bitcoin-Node-Validation-Project-Linux-VM/
├── .gitignore               # ⬅️ ESSENTIAL: Project-specific ignore rules
├── README.md                # ⬅️ ESSENTIAL: Project summary and Table of Contents
├── LICENSE
|
├── config/                  # Configuration Files (System Admin / Security)
│   ├── bitcoin.conf         # Template for Bitcoin Core (CENSOR SECRETS)
│   ├── eps_config.ini       # Template for EPS (CENSOR SECRETS & XPUB)
│   ├── bitcoind.service     # systemd file for Bitcoin Core
│   └── eps.service          # systemd file for EPS
|
├── docs/                    # Technical Documentation & Logs (Communication / Security)
│   ├── Deployment_Guide.md  # Step-by-step guide for VM and OS setup (Phase 0/1)
│   ├── Network_Security.md  # Detail on pfSense rules, Tor config, and UFW
│   ├── Troubleshooting_Log.md # Log of issues encountered and resolutions (PAR)
│   └── Security_Config.md   # Documentation of Git SSH/Passphrase and global ignores
|
├── scripts/                 # Automation and Utility Scripts (DevOps / Scripting)
│   ├── status_check.sh      # Bash script to query node health/status
│   └── install_deps.sh      # Optional: Script for initial Python/Package install
|
└── screenshots/             # Visual Proof of Work (Evidence)
    ├── 01_vm_provisioning.png # Proof of CPU/RAM/Disk allocation
    ├── 02_network_bridged.png # Proof of Bridged/Internal Network setup
    ├── 03_pfsense_firewall.png # Screenshot of pfSense firewall rules
    ├── 04_systemd_status.png  # Proof of services running via systemctl status
    └── 05_wallet_connected.png# Final proof of wallet connected to your own node
