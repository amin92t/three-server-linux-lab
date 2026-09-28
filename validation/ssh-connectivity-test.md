# SSH Connectivity Test

Status: Completed

This document records the successful SSH connectivity validation completed between the virtual machines in the Linux lab environment.

The internal lab network uses the subnet `192.168.56.0/24`.

| Hostname | Role | Operating System | Network Interface | IP Address |
|---|---|---|---|---|
| gateway | Gateway and network router | Ubuntu Server 26.04.1 LTS | ens32 | 192.168.46.129/24 |
| gateway | Gateway and network router | Ubuntu Server 26.04.1 LTS | ens33 | 192.168.56.10/24 |
| app01 | Application server | Fedora Linux 44 Workstation Edition | ens160 | 192.168.56.20/24 |
| monitor01 | Monitoring server | Ubuntu Server 26.04.1 LTS | ens33 | 192.168.56.30/24 |

SSH connectivity was tested successfully between the available Linux virtual machines.

The following checks were completed:

- IP connectivity between the virtual machines was verified.
- SSH service access between the servers was tested.
- Remote SSH login was successful.
- The default SSH port, TCP port `22`, was reachable where required.
- The SSH test confirmed that remote administration between the lab servers is available.

Example SSH commands used during validation:

```bash
ssh <username>@192.168.56.10
ssh <username>@192.168.56.20
ssh <username>@192.168.56.30
```

Expected result:

```text
After successful authentication, a remote shell session is opened on the target server.
```

Useful commands for future SSH troubleshooting:

```bash
ping -c 4 <target-ip>
ssh -v <username>@<target-ip>
sudo systemctl status sshd
sudo ss -tulpn | grep :22
sudo firewall-cmd --list-all
sudo ufw status
```

Security note:

SSH access should only be allowed for trusted users and required source networks. In a later phase of this project, password-based SSH authentication can be replaced or strengthened with SSH key-based authentication.