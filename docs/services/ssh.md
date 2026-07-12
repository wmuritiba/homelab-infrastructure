# SSH Service

## Overview

SSH (Secure Shell) provides secure remote access to the Ubuntu Server, allowing command-line administration over the network.

This is the first service deployed in the Home Lab infrastructure and serves as the foundation for all future administration tasks.

---

## Objectives

- Enable secure remote administration.
- Reduce the need for physical access to the server.
- Prepare the environment for future service deployments.

---

## Environment

| Component | Details |
|-----------|---------|
| Server | Intel NUC8 i3 |
| Operating System | Ubuntu Server 26.04 LTS |
| Service | OpenSSH Server |

---

## Installation

Update package information:

```bash
sudo apt update
```

Install OpenSSH Server:

```bash
sudo apt install openssh-server
```

Enable the service:

```bash
sudo systemctl enable ssh
```

Start the service:

```bash
sudo systemctl start ssh
```

---

## Verification

Check service status:

```bash
sudo systemctl status ssh
```

Verify that SSH is listening on port 22:

```bash
sudo ss -tulpn | grep ssh
```

Confirm the server IP address:

```bash
ip addr
```

Test the connection from another computer:

```bash
ssh username@server-ip
```

---

## Initial Configuration

Current configuration:

- Default SSH port (22)
- Password authentication enabled
- Root login: Default Ubuntu configuration

Future improvements:

- Disable root login
- Enable key-based authentication
- Disable password authentication
- Configure Fail2Ban
- Change SSH port (optional)

---

## Security Considerations

Current security level:

- SSH service enabled
- Firewall configuration pending

Planned hardening:

- SSH keys only
- Disable root login
- Install Fail2Ban
- Configure UFW firewall rules

---

## Troubleshooting

### Service not running

Symptoms:

```
Connection refused
```

Solution:

```bash
sudo systemctl start ssh
```

---

### SSH not enabled after reboot

Solution:

```bash
sudo systemctl enable ssh
```

---

### Cannot connect remotely

Verify:

- Server IP address
- Network connectivity
- SSH service status
- Firewall rules

---

## Lessons Learned

This deployment introduced the first remote management service into the Home Lab environment.

Key concepts reinforced:

- Linux service management
- systemd
- Remote administration
- Network service verification
