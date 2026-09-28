# Three-Server Linux Lab

A documented three-server Linux lab built on VMware. This project demonstrates Linux system administration, static network configuration, DNS configuration, local hostname resolution, connectivity validation, troubleshooting, and Git/GitHub documentation.

## Project Objectives

- Build a small multi-server Linux environment on VMware.
- Configure static IPv4 addressing for an internal lab network.
- Configure a gateway host with upstream and internal network interfaces.
- Configure DNS resolvers and local hostname resolution.
- Validate network communication between Linux hosts.
- Document the complete lab setup as a reproducible portfolio project.
- Use Git and GitHub to manage and publish technical documentation.

## Lab Environment

| Hostname | Role | Operating System | Interface | IP Address |
|---|---|---|---|---|
| `gateway` | Gateway and upstream-facing node | Ubuntu Server 26.04.1 LTS | `ens32` | `192.168.46.129/24` (DHCP) |
| `gateway` | Gateway and internal network node | Ubuntu Server 26.04.1 LTS | `ens33` | `192.168.56.10/24` (Static) |
| `app01` | Application node | Fedora Linux 44 Workstation Edition | `ens160` | `192.168.56.20/24` (Static) |
| `monitor01` | Monitoring and administration node | Ubuntu Server 26.04.1 LTS | `ens33` | `192.168.56.30/24` (Static) |

## Network Overview

### Internal Lab Network

```text
Network: 192.168.56.0/24
Gateway: 192.168.56.10
```

### Gateway Upstream Network

```text
Network: 192.168.46.0/24
Interface: ens32
Address: 192.168.46.129/24
Address Assignment: DHCP
Upstream Default Gateway: 192.168.46.2
```

### DNS Servers

```text
1.1.1.1
8.8.8.8
```

## Network Topology

```text
                         Upstream Network
                         192.168.46.0/24
                                |
                     Default Gateway: 192.168.46.2
                                |
                gateway - Ubuntu Server 26.04.1 LTS
                ens32: 192.168.46.129/24 (DHCP)
                ens33: 192.168.56.10/24  (Static)
                                |
             Internal Lab Network: 192.168.56.0/24
                    /                           \
                   /                             \
                  /                               \
      app01 - Fedora 44                    monitor01 - Ubuntu 26.04
      ens160: 192.168.56.20/24             ens33: 192.168.56.30/24
      Role: Application Node                Role: Monitoring Node
```

## Hostname Resolution

The following local hostname mappings are configured on the lab hosts:

```text
192.168.56.10 gateway
192.168.56.20 app01
192.168.56.30 monitor01
```

## Repository Structure

```text
three-server-linux-lab/
├── README.md
├── .gitignore
├── docs/
│   ├── project-overview.md
│   ├── ip-plan.md
│   ├── network-design.md
│   ├── host-configuration.md
│   ├── dns-and-hosts.md
│   └── lab-build-runbook.md
├── diagrams/
│   └── network-topology.md
├── configs/
│   ├── gateway-netplan.yaml
│   ├── monitor01-netplan.yaml
│   ├── app01-networkmanager.md
│   └── hosts-file-example.txt
├── validation/
│   ├── connectivity-tests.md
│   └── validation-checklist.md
├── troubleshooting/
│   └── dns-troubleshooting.md
└── screenshots/
    └── README.md
```

## Technologies Used

- VMware
- Ubuntu Server 26.04.1 LTS
- Fedora Linux 44 Workstation Edition
- Netplan
- NetworkManager
- systemd-resolved
- ping
- ssh
- curl
- tcpdump
- Git
- GitHub

## Validation Scope

The lab validation includes the following checks:

- Verify static IP addresses on internal interfaces.
- Verify the default route on `app01` and `monitor01`.
- Verify connectivity between all lab hosts using `ping`.
- Verify hostname resolution using `/etc/hosts`.
- Verify DNS resolver configuration.
- Verify external network connectivity.
- Verify SSH connectivity between lab hosts where SSH is enabled.
- Document and troubleshoot DNS resolution issues.

## Documentation

- `docs/` - Project overview, IP plan, network design, host configuration, and build runbook.
- `diagrams/` - Network topology diagrams.
- `configs/` - Sanitized configuration examples.
- `validation/` - Connectivity tests and validation checklist.
- `troubleshooting/` - DNS troubleshooting notes and issue analysis.
- `screenshots/` - Sanitized screenshots from the VMware lab environment.

## Security Notice

This repository contains sanitized lab documentation only.

The following items must never be committed to Git or GitHub:

- Passwords
- API tokens
- Private SSH keys
- Certificates
- VPN configuration files
- Personal data
- Sensitive production configuration files

## Author

Created as a hands-on Linux system administration and networking lab project.