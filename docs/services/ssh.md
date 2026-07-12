# SSH Service

## Overview

SSH (Secure Shell) provides secure remote administration for the Home Lab server, allowing management from an authorized workstation without requiring physical access.

---

## Objective

Provide secure remote administration for the Ubuntu Server.

---

## Environment

| Component | Details |
|-----------|---------|
| Server | Intel NUC8i3BEH |
| Operating System | Ubuntu Server 26.04 LTS |
| Client | Windows 11 |
| SSH Client | OpenSSH |

---

## Implementation

### Install OpenSSH Server

```bash
sudo apt update
sudo apt install openssh-server
```

Enable the service:

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

---

## Validation

Remote administration was successfully validated from a Windows workstation using the OpenSSH client.

### SSH Login

<p align="center">
  <img
    src="../../assets/screenshots/ssh-login.png"
    alt="Successful SSH connection from a Windows workstation"
    width="700"
  >
</p>

<p align="center">
  <em>Figure 1 — Successful SSH connection from a Windows workstation.</em>
</p>

---
### System Information

<p align="center">
  <img
    src="../../assets/screenshots/system-information.png"
    alt="Ubuntu Server system information"
    width="650"
  >
</p>

<p align="center">
  <em>Figure 2 — Ubuntu Server system information and hardware details.</em>
</p>

## Configuration

| Setting | Value |
|---------|-------|
| Port | 22 |
| Authentication | Password |
| Service | Enabled |
| Startup | Automatic |

---

## Security

Current implementation:

- Password authentication enabled
- Default SSH port (22)

Future improvements:

- SSH key authentication
- Disable password authentication
- Disable root login
- Fail2Ban
- UFW firewall rules

---

## Troubleshooting

### Issue

Unable to authenticate during the first remote login.

### Investigation

- SSH service was reachable.
- Network connectivity verified.
- Authentication prompt displayed correctly.

### Resolution

Recovered the correct credentials and successfully authenticated.

---

## Lessons Learned

- Always validate SSH immediately after installing the operating system.
- Store credentials securely in a password manager.
- Verify connectivity before assuming the SSH service is unavailable.
