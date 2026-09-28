cat > docs/ip-plan.md <<'EOF'
# IP Address Plan

This document defines the IP addressing plan for the three-server Linux lab.

## Internal Lab Network

| Network | CIDR | Description |
|---|---|---|
| `192.168.56.0` | `/24` | Internal network used for communication between the gateway, application, and monitoring hosts |

## Host Addressing

| Hostname | Role | Operating System | Interface | IP Address | Address Assignment |
|---|---|---|---|---|---|
| `gateway` | Gateway node | Ubuntu Server 26.04.1 LTS | `ens32` | `192.168.46.129/24` | DHCP |
| `gateway` | Internal gateway interface | Ubuntu Server 26.04.1 LTS | `ens33` | `192.168.56.10/24` | Static |
| `app01` | Application node | Fedora Linux 44 Workstation Edition | `ens160` | `192.168.56.20/24` | Static |
| `monitor01` | Monitoring and administration node | Ubuntu Server 26.04.1 LTS | `ens33` | `192.168.56.30/24` | Static |

## Internal Network Gateway

| Setting | Value |
|---|---|
| Internal network | `192.168.56.0/24` |
| Gateway hostname | `gateway` |
| Gateway IP address | `192.168.56.10` |
| Clients using this gateway | `app01`, `monitor01` |

## DNS Resolver Configuration

| DNS Server | Provider |
|---|---|
| `1.1.1.1` | Cloudflare DNS |
| `8.8.8.8` | Google Public DNS |

## Local Hostname Resolution

| IP Address | Hostname |
|---|---|
| `192.168.56.10` | `gateway` |
| `192.168.56.20` | `app01` |
| `192.168.56.30` | `monitor01` |

## Addressing Notes

- The `192.168.56.0/24` network is dedicated to the internal lab environment.
- The `gateway` host connects the internal lab network to the upstream network through its `ens32` interface.
- The upstream address on `gateway` (`192.168.46.129/24`) is assigned using DHCP.
- The internal addresses on `gateway`, `app01`, and `monitor01` are statically configured.
- `app01` and `monitor01` use `192.168.56.10` as their default gateway for traffic outside the internal network.
EOF