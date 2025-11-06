# 🧠 SOC & Cloud Lab / thread-blue Systems Data Center

**Purpose:**  
This repository documents the design and automation of a hybrid **SOC + Cloud Architecture Lab**, used for security operations, DevNet automation, and AI experimentation.

---

## ✅ Immediate To-Do

| 🔴 **High Priority (Immediate)** | 🟠 **Medium Priority (Soon)** | 🟢 **Low Priority (Later)** |
|:--|:--|:--|
| 🔴 Document Switch — VLAN map + port assignments | 🟠 Finish Terraform + AWS CLI integration | 🟢 Add cloud visualization diagrams |
| 🔴 Finalize Ubuntu template VM → Proxmox template | 🟠 Deploy Nextcloud (500 GB → 2 TB pool) | 🟢 Build CI/CD workflow for IaC repos |
| 🔴 ZFS replication (lux → pve + off-site S3 backups) | 🟠 Configure Ansible playbooks | 🟢 Refine monitoring dashboard layout |
| 🔴 Create LLM node container (RTX 3090) | 🟠 Deploy local DevNet lab | 🟢 Expand Tailscale ACL policy |
| 🔴 Clone daily-driver VMs (Win11 + Ubuntu) | 🟠 Add Python automation utilities | 🟢 Build offline exam OS profile |

---

## 🏗️ Infrastructure Overview

### 📶 Core Network Stack

| Layer | Device | Role / Notes |
|:--|:--|:--|
| **Router** | Starlink Router | Internet uplink and default gateway; feeds Cisco switch |
| **Switch** | Cisco Catalyst 2960-S | Managed L2 switch with 802.1Q VLAN tagging |
| **VPN Mesh** | Tailscale | Peer-to-peer VPN mesh between mobile and lab nodes |
| **Admin Devices** | 2 × M1 Max MacBooks | Proxmox admin + remote streaming via Sunshine/Moonlight |
Starlink Router
↓
Cisco 2960-S Switch (CoreSwitch)
├── lux (Main Node)
├── pve (Backup Node)
└── LAN Devices / VPN
---

### 🖥️ Main Compute Node (`lux`)

| Component | Details |
|:--|:--|
| **CPU** | Xeon Gold 5120 → planned upgrade: Xeon Gold 6252 |
| **RAM** | 64 GB DDR4 |
| **Storage A** | 3 × 500 GB SSD (RAIDZ-1 ≈ 1 TB usable, ZFS encrypted) |
| **Storage B** | NVMe 256 GB boot (encrypted) |
| **Storage C** | SATA 256 GB for offline OS testing |
| **GPU 1** | Radeon HD 5770 → planned RTX 4060 Ti (16 GB) |
| **GPU 2** | GTX 1080 → RTX 3090 (24 GB) |
| **OS** | Proxmox VE 8.2 (Type-1 hypervisor) |
| **Usage** | AI/LLM workloads, Windows gaming VMs, cloud automation |
| **Security** | VLAN segmentation · ZFS encryption · 2FA login |

---

### 🖥️ Backup / Storage Node (`pve`)

| Component | Details |
|:--|:--|
| **CPU** | i7-950 → planned Xeon X5670 |
| **RAM** | 12 GB |
| **Storage** | 4 × HDD RAID 10 (≈ 1.8 TB usable) |
| **Role** | Backups, templates, Nextcloud host |
| **Notes** | Target for ZFS replication from `lux` |

---

### 📱 Mobile Thin Client

| Component | Details |
|:--|:--|
| **Device** | Samsung Galaxy S25 Ultra |
| **OS** | Android 15 + Knox Work Profile |
| **VPN** | Always-on AES-256 Tailscale |
| **Display** | 6.8″ + 7″ portable HDMI |
| **Use** | Remote admin console for Proxmox and cloud monitoring |

**Security Notes:**  
Firmware locked and patched · VPN required before access · No sideloaded apps · Casting only on wired HDMI or WPA3 LAN

---

## ☁️ Hybrid Cloud & Virtual Machine Plan

| Priority | VM / Container | Purpose / Use | Storage Target | Status |
|:--|:--|:--|:--|:--|
| 🔴 | **win11-home-1** | Primary Windows VM (gaming + daily use) | SSD RAIDZ-1 | Active |
| 🔴 | **ubuntu-dev-1** | Main Linux VM (coding, IaC testing) | NVMe | Active |
| 🔴 | **win11-home-2** | Second user VM (shared access) | SSD RAIDZ-1 | Pending |
| 🔴 | **ubuntu-dev-2** | Secondary Linux VM (for spouse) | NVMe | Pending |
| 🔴 | **nextcloud-srv** | Local private cloud (500 GB → 2 TB) | HDD RAID10 | Planned |
| 🔴 | **llm-node** | Local AI inference (3090 GPU) | NVMe | Planned |
| 🟠 | **wazuh-mgr** | SOC SIEM management | SSD | Planned |
| 🟠 | **suricata-ids** | Network IDS sensor | SSD | Planned |
| 🟠 | **pihole-dns** | DNS filtering + ad blocker | SSD | Active |
| 🟢 | **terraform-ctl** | IaC control node → AWS Free Tier | Shared | Optional |
| 🟢 | **aws-ec2-test** | Remote bastion host (Free Tier t2.micro) | AWS | Optional |
| 🟢 | **malware-lab** | Isolated sandbox environment | HDD RAID10 | Optional |

**Color Key:**  
🔴 = Immediate build 🟠 = Next phase 🟢 = Later/optional

---

## 🧰 Software & Security Stack

| Layer | Tool | Purpose |
|:--|:--|:--|
| **Hypervisor** | Proxmox VE 8.2 | Virtualization core |
| **Storage** | ZFS (Encrypted) | Pool management + replication |
| **Containers** | Docker / Compose | Service orchestration |
| **IDS / SIEM** | Suricata + Wazuh | Security monitoring |
| **Visualization** | Grafana + Loki | Metrics and logs |
| **DNS Filtering** | Pi-hole | Malware / Ad blocking |
| **VPN Mesh** | Tailscale | Secure remote connectivity |
| **Automation** | Terraform / Ansible / Python | IaC and system patching |
| **Cloud Services** | AWS Free Tier | Off-site backup + IaC sandbox |

---

## 📂 Repository Structure
soc-cloud-lab/
├── infrastructure/
│   ├── network-topology.md
│   ├── hardware-specs.md
│   ├── vm-profiles/
│   ├── proxmox-configs/
│   └── zfs-replication.md
│
├── projects/
│   ├── 01-local-speedtest-server/
│   ├── 02-wazuh-siem/
│   ├── 03-suricata-ids/
│   ├── 04-dns-filter-pihole/
│   ├── 05-internal-malware-lab/
│   ├── 06-devnet-automation-lab/
│   └── 07-local-llm-node/
│
├── cloud/
│   ├── aws-free-tier/
│   ├── hybrid-infra/
│   └── visualization/
│
├── docs/
│   ├── study-roadmap.md
│   ├── security-policies.md
│   ├── internship-notes.md
│   ├── ai-for-it-automation.md
│   └── aws-capstone-project.md
│
└── assets/
├── diagrams/
└── screenshots/
---

## 🧭 Study & Certification Roadmap

| Phase | Focus | Certifications |
|:--|:--|:--|
| **Phase 1** | Core Security + Networking | CompTIA (A+ / N+ / Sec+) ✅ |
| **Phase 2** | Cloud Architecture | AWS SAA |
| **Phase 3** | Network Automation | Cisco DevNet Associate |
| **Phase 4** | **Bachelor’s Completion + Applied Security** | Final units + AWS SAP + DevNet |
| **Phase 5** | Advanced Security | Pentest+ → CASP+ → CISM |
| **Phase 6** | Graduate Studies | Master’s in Cybersecurity |
| **Phase 7** | AI & Automation | AI for IT Automation · Network Analytics · IaC Security |

---

## ⚙️ Next Steps
- [ ] 🔴 Finalize Proxmox templates and replication  
- [ ] 🔴 Build Nextcloud instance (500 GB → 2 TB ZFS)  
- [ ] 🔴 Deploy LLM Node (Ollama / LM Studio)  
- [ ] 🟠 Terraform + AWS integration testing  
- [ ] 🟠 Ansible playbooks for VM updates  
- [ ] 🟢 Grafana + Loki SOC dashboard  

---

## 🧩 Summary
> The **thread-blue Systems Lab** is a full hybrid environment for applied security, automation, and cloud research.  
> It showcases hands-on capability with:
> - SOC analysis and threat response  
> - Infrastructure as Code (IaC) + Automation  
> - Cloud architecture integration  
> - Local AI and data sovereignty projects  

---

## 🔒 Public Safety Check
- No private keys, credentials, or IPs exposed.  
- Hardware specs are generic and safe to share.  
- Cloud IDs, VPN keys, and domain details intentionally omitted.  
- Safe for public viewing and portfolio use.
