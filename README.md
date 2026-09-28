# Three-Server Linux Lab: Networking & Administration

This project is a hands-on lab environment I built using VMware to practice and demonstrate core Linux system administration and networking principles. The goal was to create a reproducible, multi-node setup that mimics a real-world small-scale data center environment.

## 🛠 Project Overview
In this lab, I configured a multi-server network focusing on static addressing, gateway routing, and DNS resolution. This wasn't just about connectivity; it was a deep dive into troubleshooting and managing different Linux distributions in a single ecosystem.

### Core Objectives:
* **Network Architecture:** Designed and implemented a static IPv4 internal network.
* **Gateway Implementation:** Configured a custom gateway node to handle routing between upstream (DHCP) and internal (Static) networks.
* **Service Configuration:** Set up DNS resolvers and local hostname resolution to ensure seamless communication.
* **Automation & Documentation:** Managed the entire setup using Git/GitHub, documenting every step for reproducibility.

## 🖥 Lab Environment & Topology

### Node Configuration
| Hostname | Role | Operating System | Interface | IP Address |
| :--- | :--- | :--- | :--- | :--- |
| **gateway** | Network Gateway | Ubuntu 26.04.1 LTS | ens32 (Upstream)<br>ens33 (Internal) | 192.168.46.129 (DHCP)<br>192.168.56.10 (Static) |
| **app01** | Application Node | Fedora 44 | ens160 | 192.168.56.20 (Static) |
| **monitor01** | Monitoring Node | Ubuntu 26.04.1 LTS | ens33 | 192.168.56.30 (Static) |

### Network Layout
The architecture follows a standard gateway-client model:
* **Upstream Network:** `192.168.46.0/24` (via Default Gateway `192.168.46.2`)
* **Internal Lab Network:** `192.168.56.0/24` (Managed by the `gateway` node)
* **DNS Providers:** `1.1.1.1`, `8.8.8.8`

---

## 📂 Repository Structure
I've organized this repository to serve as a complete "Runbook." You can find everything from raw configurations to troubleshooting logs:

* `docs/` — Detailed guides on IP planning, network design, and build instructions.
* `configs/` — Sanitized configuration files (Netplan, NetworkManager, etc.).
* `validation/` — My checklist and test results used to verify the lab's stability.
* `troubleshooting/` — A log of DNS issues encountered and how I resolved them.
* `screenshots/` — Visual confirmation of the lab setup.

## 🧪 Validation & Testing
To ensure the environment was production-ready, I performed the following checks:
- [x] Verified static IP assignment and default routes on all nodes.
- [x] Confirmed inter-node connectivity via `ping`.
- [x] Tested hostname resolution via `/etc/hosts` and DNS.
- [x] Validated external internet access through the gateway.
- [x] Tested SSH connectivity and service reachability.

## ⚠️ Security Note
**Important:** All configuration files in this repository have been **sanitized**. I have strictly ensured that no sensitive data (passwords, private SSH keys, or API tokens) is committed to this repository.

---
*Created as a hands-on Linux system administration and networking lab project.*
