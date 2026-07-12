# SSH Service

## Overview

SSH (Secure Shell) provides secure remote access to the Ubuntu Server, allowing administration from another computer without requiring physical access.

This is the first service deployed in the Home Lab environment.

---

# Objective

Provide secure remote administration for the Home Lab server.

---

# Architecture

Client (Windows 11)

↓

SSH (TCP/22)

↓

Ubuntu Server

↓

Intel NUC8 i3

---

# Implementation

### Install OpenSSH Server

```bash
sudo apt update
sudo apt install openssh-server
```

### Enable the service

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

---

# Validation

Service status

```bash
sudo systemctl status ssh
```

Listening port

```bash
ss -tulpn | grep ssh
```

Remote connection

```powershell
ssh username@server-ip
```

Result

✅ Successfully connected from Windows.

---

# Current Configuration

| Setting | Value |
|---------|-------|
| Port | 22 |
| Authentication | Password |
| Root Login | Default |
| Startup | Enabled |

---

# Future Improvements

- Disable root login
- Configure SSH keys
- Disable password authentication
- Configure UFW rules
- Install Fail2Ban

---

# Troubleshooting

## Issue

Unable to log in remotely.

### Investigation

- SSH service responded correctly.
- Network connectivity verified.
- Authentication prompt displayed.

### Root Cause

Incorrect password.

### Resolution

Recovered the correct credentials and successfully authenticated.

---

# Lessons Learned

- Verify SSH connectivity before assuming the service is unavailable.
- Document deployment credentials in a secure password manager.
- Test remote access immediately after installation.

- Linux service management
- systemd
- Remote administration
- Network service verification
