cat > docs/lab-build-runbook.md <<'EOF'
# Three-Server Linux Lab Build Runbook

This runbook documents the initial deployment, network design, baseline configuration, and validation process for the three-server Linux lab.

## Lab Purpose

This lab is designed to practice Linux administration, static network configuration, hostname resolution, DNS troubleshooting, routing, NAT, SSH connectivity, documentation, and Git.

## Virtual Machines

| Hostname | Role | Operating System |
|---|---|---|
| `gateway` | Gateway between the internal lab network and the upstream network | Ubuntu Server 26.04.1 LTS |
| `app01` | Application server | Fedora Linux 44 Workstation Edition |
| `monitor01` | Monitoring and administration server | Ubuntu Server 26.04.1 LTS |

## Network Design

The environment contains two networks.

| Network | CIDR | Purpose |
|---|---|---|
| Upstream network | `192.168.46.0/24` | Provides upstream connectivity to the gateway host |
| Internal lab network | `192.168.56.0/24` | Connects `gateway`, `app01`, and `monitor01` |

The `gateway` host has two network interfaces. One interface is connected to the upstream network, and the second interface is connected to the isolated internal lab network.

## Interface Configuration

| Hostname | Interface | IP Address | Address Assignment | Purpose |
|---|---|---|---|---|
| `gateway` | `ens32` | `192.168.46.129/24` | DHCP | Upstream network connection |
| `gateway` | `ens33` | `192.168.56.10/24` | Static | Internal lab network gateway |
| `app01` | `ens160` | `192.168.56.20/24` | Static | Internal lab network connection |
| `monitor01` | `ens33` | `192.168.56.30/24` | Static | Internal lab network connection |

## Default Gateway Configuration

The internal hosts must send traffic outside the internal network to `gateway`.

| Hostname | Default Gateway |
|---|---|
| `app01` | `192.168.56.10` |
| `monitor01` | `192.168.56.10` |

## DNS Resolver Configuration

The lab hosts use the following public DNS resolvers.

| DNS Server | Provider |
|---|---|
| `1.1.1.1` | Cloudflare DNS |
| `8.8.8.8` | Google Public DNS |

## Local Hostname Resolution

The following entries must exist in the `/etc/hosts` file on `gateway`, `app01`, and `monitor01`.

    192.168.56.10 gateway
    192.168.56.20 app01
    192.168.56.30 monitor01

These entries allow the hosts to resolve internal lab names even when external DNS is unavailable.

## Hostname Configuration

Each virtual machine must use its assigned hostname.

| Hostname | Command |
|---|---|
| `gateway` | `sudo hostnamectl set-hostname gateway` |
| `app01` | `sudo hostnamectl set-hostname app01` |
| `monitor01` | `sudo hostnamectl set-hostname monitor01` |

Verify the configured hostname:

    hostnamectl

## Baseline Tools

The following command-line tools are required for administration and troubleshooting.

| Tool | Purpose |
|---|---|
| `curl` | Test HTTP and HTTPS connectivity |
| `vim` | Edit configuration files |
| `git` | Track project files and documentation |
| `net-tools` | Provide legacy commands such as `ifconfig` and `netstat` |
| `tcpdump` | Capture and inspect network traffic |

Install the required tools on Ubuntu hosts:

    sudo apt update
    sudo apt install -y curl vim git net-tools tcpdump

Install the required tools on the Fedora host:

    sudo dnf install -y curl vim git net-tools tcpdump

## Basic Network Verification

Verify the IP addresses assigned to network interfaces:

    ip address

Verify the active routing table:

    ip route

Verify connectivity from `monitor01` to the internal gateway and application host:

    ping -c 4 192.168.56.10
    ping -c 4 192.168.56.20

Verify connectivity from `app01` to the internal gateway and monitoring host:

    ping -c 4 192.168.56.10
    ping -c 4 192.168.56.30

Verify connectivity from `gateway` to both internal hosts:

    ping -c 4 192.168.56.20
    ping -c 4 192.168.56.30

Verify local hostname resolution:

    getent hosts gateway
    getent hosts app01
    getent hosts monitor01

Verify the configured DNS resolver on Ubuntu systems:

    resolvectl status

Verify the configured DNS resolver on Fedora:

    cat /etc/resolv.conf

Verify external IP connectivity:

    ping -c 4 8.8.8.8

Verify external DNS resolution and HTTPS access:

    ping -c 4 google.com
    curl -I https://github.com

## Expected Results

The following results are expected after baseline configuration is complete:

- `gateway`, `app01`, and `monitor01` can ping each other through the `192.168.56.0/24` network.
- The hostname `gateway` resolves to `192.168.56.10`.
- The hostname `app01` resolves to `192.168.56.20`.
- The hostname `monitor01` resolves to `192.168.56.30`.
- `app01` and `monitor01` use `192.168.56.10` as their default gateway.
- DNS resolvers are configured as `1.1.1.1` and `8.8.8.8`.
- Internal hostname resolution works through `/etc/hosts`.
- External IP connectivity to `8.8.8.8` requires IP forwarding and NAT to be configured on `gateway`.
- External name resolution for domains such as `google.com` requires both a working route through `gateway` and a working DNS resolver.
- HTTPS access to GitHub through `curl -I https://github.com` requires routing, NAT, DNS, and internet connectivity to work correctly.

## Troubleshooting Notes

If a host can ping `8.8.8.8` but cannot ping or curl a domain name such as `google.com` or `github.com`, the problem is DNS resolution.

Useful DNS troubleshooting commands:

    cat /etc/resolv.conf
    resolvectl status
    getent hosts github.com
    getent hosts google.com

If internal hosts can communicate with each other but cannot reach `8.8.8.8`, verify the default route on `app01` and `monitor01`:

    ip route

The expected default route on internal hosts is:

    default via 192.168.56.10

If internal hosts use the correct default route but cannot access the internet, verify that IP forwarding and NAT are enabled on `gateway`.

Check IP forwarding:

    sysctl net.ipv4.ip_forward

The expected value is:

    net.ipv4.ip_forward = 1


EOF