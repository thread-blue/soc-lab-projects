![GitHub last commit](https://img.shields.io/github/last-commit/johnnythegra/johnnylabs-soclab?style=flat-square)
![GitHub repo size](https://img.shields.io/github/repo-size/johnnythegra/johnnylabs-soclab?style=flat-square)
![GitHub stars](https://img.shields.io/github/stars/johnnythegra/johnnylabs-soclab?style=flat-square)
![GitHub license](https://img.shields.io/github/license/johnnythegra/johnnylabs-soclab?style=flat-square)

> **Repository Purpose:**  
> Documentation and configuration of Johnny’s self-hosted SOC (Security Operations Center) lab — running on Proxmox VE with network monitoring, AI workloads, and full VLAN isolation.

---

## 📜 Lab Activity Log
- **2025-08-09** — Added Local Speedtest Server VM under `projects/01-local-speedtest-server`
- **2025-08-07** — Updated Proxmox to 8.2 and enabled 2FA for admin
- **2025-08-05** — Integrated Suricata IDS with Wazuh SIEM (initial alert testing)
- **2025-08-03** — Expanded VLAN segmentation to isolate legacy systems
- **2025-08-01** — Documented Samsung S25 Ultra thin-client setup in README

---

## ✅ Immediate To-Do
1. **Document Switch** — record port assignments + VLAN map for Cisco 2960-S  
2. **Template VM** — finalize Ubuntu base image → Proxmox template  
3. **GitHub Sync** — configure SSH key and push repo updates  
4. **ZFS Replication** — automate lux → pve + off-site AWS S3 backups  
5. **Network Diagram** — finalize topology in `network-topology.md`  
6. **Monitoring Pipeline** — link Suricata → Wazuh → Dashboard  
7. **Study Roadmap** — Security+ → AWS SAA → ISO 27001 Implementer  
8. **Self-Hosted Cloud** — deploy Nextcloud or MinIO on pve  
9. **Tailscale Policy** — define ACLs for MacBooks + Samsung access  
10. **Snapshot Automation** — schedule VM snapshots and offload to ZFS backup  

---

## 🏗️ Current Infrastructure Overview

### 📶 Core Network Stack

| Layer | Device | Role / Notes |
|:--|:--|:--|
| **Router** | **Starlink Router** | Internet uplink and default gateway; feeds Cisco switch |
| **Switch** | **Cisco Catalyst 2960-S** | Managed L2 switch with 802.1Q VLAN tagging; connects both Proxmox nodes |
| **VPN Mesh** | **Tailscale** | Secure site-to-site mesh between mobile devices and lab |
| **Management Access** | **2× M1 Max MacBooks** | Access Proxmox via Tailscale using root credentials + 2FA; also handle Sunshine/Moonlight streaming |

**Network Diagram (Simplified):**
Starlink Router
↓
Cisco 2960-S Switch
├── lux (Main Node)
├── pve (Backup Node)
└── LAN Devices / VPN
---

### 🖥️ Headless Server (Main Compute Node — `lux`)

| Component | Details |
|:--|:--|
| **CPU** | Xeon Gold 5120 → *planned upgrade:* Xeon Gold 6252 |
| **RAM** | 64 GB DDR4 |
| **Storage A** | 3 × 500 GB SSD in RAIDZ-1 (~1 TB usable, ZFS encrypted) |
| **Storage B** | NVMe 256 GB |
| **Storage C** | 256 GB SATA SSD *(dedicated for offline OS during proctored exams)* |
| **GPU 1** | Radeon HD 5770 → RTX 4060 Ti (16 GB) |
| **GPU 2** | GTX 1080 → RTX 3090 (24 GB) |
| **OS** | Proxmox VE (Type-1 Hypervisor) + Windows 11 VM (isolated) |
| **Security** | VLAN segmentation, ZFS encryption, 2FA (login), long-form root password |
| **Access** | Local network IP login + 2FA (no SSH keys currently configured) |
| **Display** | Single 42″ monitor → *second screen planned* |
| **Usage** | AI / LLM workloads, Windows gaming VM (Sunshine → Moonlight), offline testing environment |

---

### 🖥️ Backup / Storage Node (`pve`)

| Component | Details |
|:--|:--|
| **CPU** | i7-950 → *planned upgrade:* Xeon X5670 |
| **RAM** | 12 GB |
| **Storage** | 4 × HDD in RAID 10 (~1.8 TB usable) |
| **Role** | Backups, long-term storage, Proxmox templates |
| **Notes** | RAID 10 provides redundancy + fast ZFS replication from `lux` |

---

### 📱 Workstation / Thin Client  
**Device:** Samsung Galaxy S25 Ultra — Hardened Mobile Thin Client

| Component | Details |
|:--|:--|
| **OS** | Android 15 + Samsung Knox Work Profile |
| **VPN** | Always-on AES-256 tunnel (Tailscale endpoint) |
| **Access** | Requires network + 2FA root login (no SSH keys) |
| **Display** | Dual-screen setup (6.8″ main + 7″ portable HDMI) |
| **Role** | Portable SOC / Admin console for Proxmox management |

**Security Notes**
- Knox profile isolates work vs personal apps  
- VPN required before server access  
- Casting only allowed on wired HDMI or WPA3 LANs  
- No sideloaded apps / firmware always patched  

---

## 🧰 Software & Security Stack

| Layer | Tool | Purpose |
|:--|:--|:--|
| **Hypervisor** | **Proxmox VE 8.2** | Type-1 virtualization layer |
| **Storage** | **ZFS (Encrypted)** | Disk pooling + replication |
| **Containerization** | **Docker + Compose** | Lightweight service deployment |
| **IDS / SIEM** | **Suricata + Wazuh** | Network + endpoint monitoring |
| **DNS Filtering** | **Pi-hole** | Ad + malware blocking |
| **Monitoring** | **Speedtest VM** | Internal latency and throughput tracking |
| **VPN Gateway** | **Tailscale** | Secure mesh access for remote devices |

---

## 📂 Folder Structure
johnnylabs-soclab/
├── infrastructure/
│   ├── network-topology.md
│   ├── hardware-specs.md
│   └── proxmox-configs/
│       └── README.md
├── projects/
│   ├── 01-local-speedtest-server/
│   ├── 02-wazuh-siem/
│   ├── 03-suricata-ids/
│   ├── 04-dns-filter-pihole/
│   └── 05-internal-malware-lab/
├── docs/
│   ├── lab-roadmap.md
│   ├── security-policies.md
│   └── learning-notes.md
└── assets/
└── screenshots/
