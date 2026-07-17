# VPN Gateway

## Overview

This project implements a WireGuard connection to Surfshark on the Ubuntu Server.

The initial phase establishes and validates the VPN tunnel on the Intel NUC. A later phase will configure the server to forward traffic from the Samsung Frame TV through this tunnel.

---

## Objective

The objective is to use the Intel NUC as a dedicated VPN gateway for devices that do not support native VPN applications.

The implementation is divided into two phases:

1. Establish and validate the Surfshark WireGuard tunnel on the Ubuntu Server.
2. Configure IP forwarding, NAT and firewall rules so the Samsung Frame TV can use the NUC as its default gateway.

---

## Environment

| Component | Details |
|-----------|---------|
| Server | Intel NUC8i3BEH |
| Operating System | Ubuntu Server 26.04 LTS |
| VPN Provider | Surfshark |
| VPN Protocol | WireGuard |
| WireGuard Tools | v1.0.20250521 |
| VPN Location | São Paulo, Brazil |
| VPN Interface | `wg0` |
| VPN Address | `10.14.0.2/16` |
| Physical Network Interface | `wlp0s20f3` |
| Client Device | Samsung Frame TV |
| Primary Router | eero 6 |

---

## Installation

### WireGuard Packages

WireGuard and its command-line utilities were installed from the Ubuntu repositories.

```bash
sudo apt install wireguard wireguard-tools -y
```

The installed version was verified with:

```bash
wg --version
```

Result:

```text
wireguard-tools v1.0.20250521
```

---

## Configuration

### Surfshark Credentials

A WireGuard key pair was generated through the Surfshark manual configuration portal.

After selecting the São Paulo VPN location, a complete WireGuard configuration profile was downloaded.

The profile was transferred to the Ubuntu Server and installed as:

```text
/etc/wireguard/wg0.conf
```

### File Ownership and Permissions

Because the configuration file contains the WireGuard private key, access was restricted to the root user.

```bash
sudo chown root:root /etc/wireguard/wg0.conf
sudo chmod 600 /etc/wireguard/wg0.conf
```

Expected permissions:

```text
-rw------- 1 root root ... wg0.conf
```

### Configuration Structure

The deployed profile contains the following non-sensitive configuration:

```ini
[Interface]
Address = 10.14.0.2/16
DNS = 162.252.172.57, 149.154.159.92

[Peer]
AllowedIPs = 0.0.0.0/0
Endpoint = br-sao.prod.surfshark.com:51820
```

The private and public cryptographic keys are intentionally excluded from this repository.

### Tunnel Activation

The VPN tunnel was activated using:

```bash
sudo wg-quick up wg0
```

---

## Validation

### WireGuard Tunnel Status

The WireGuard status was checked to confirm that the tunnel was established successfully.

The validation confirmed:

- The `wg0` interface was active.
- A Surfshark peer was configured.
- A recent WireGuard handshake was present.
- Traffic was being transmitted and received.
- The default WireGuard policy included all IPv4 destinations.

<p align="center">
  <img
    src="../../assets/screenshots/wireguard-status.png"
    alt="Active Surfshark WireGuard tunnel"
    width="650"
  >
</p>

<p align="center">
  <em>Figure 1 — Active WireGuard tunnel with a successful Surfshark handshake.</em>
</p>

### WireGuard Network Interface

The network interface was inspected to confirm that WireGuard created the virtual interface and assigned the expected VPN address.

```bash
ip addr show wg0
```

The validation confirmed:

- The `wg0` interface was created.
- The interface was operational.
- The VPN address `10.14.0.2/16` was assigned.
- The WireGuard MTU was configured as `1420`.

<p align="center">
  <img
    src="../../assets/screenshots/wg0-interface.png"
    alt="WireGuard wg0 network interface"
    width="650"
  >
</p>

<p align="center">
  <em>Figure 2 — WireGuard virtual interface with the Surfshark VPN address assigned.</em>
</p>

### Public IP and VPN Location

The server’s public network identity was checked after activating the tunnel.

```bash
curl ipinfo.io
```

The result confirmed:

- The public IP changed after the VPN connection was established.
- The detected city and region were São Paulo.
- The detected country was Brazil.
- The network organisation was associated with the VPN service.

<p align="center">
  <img
    src="../../assets/screenshots/vpn-public-ip.png"
    alt="Public network identity after VPN activation"
    width="650"
  >
</p>

<p align="center">
  <em>Figure 3 — Public network identity associated with the Surfshark São Paulo VPN location.</em>
</p>

---

## Security Considerations

- The WireGuard configuration file is owned by `root`.
- File permissions are restricted to `600`.
- The WireGuard private key is not stored in the GitHub repository.
- Cryptographic keys are removed or masked from all screenshots.
- The public VPN address is masked in published screenshots.
- The real `wg0.conf` file must never be committed to Git.

A sanitised configuration example may be added to the repository later without private keys or provider credentials.

---

## Troubleshooting

### Incomplete Configuration Template

The first downloaded configuration contained:

```text
PrivateKey = <insert_your_private_key_here>
```

This indicated that the file was only a template and could not establish a valid WireGuard connection.

The issue was resolved by:

1. Generating a WireGuard key pair through the Surfshark portal.
2. Selecting the São Paulo VPN location.
3. Downloading the completed location-specific configuration.
4. Replacing the incomplete template on the server.

### Permission Denied When Accessing `/etc/wireguard`

The directory is restricted to the root user. Administrative commands were therefore executed with `sudo`.

Example:

```bash
sudo ls -la /etc/wireguard
```

---

## Lessons Learned

- A downloaded VPN profile should be inspected before activation to confirm that it does not contain placeholder values.
- WireGuard private keys must be protected with restrictive file ownership and permissions.
- A successful handshake confirms peer communication, but public IP validation is also required to prove that Internet traffic is using the tunnel.
- VPN connectivity should be validated on the server before configuring forwarding, NAT or client devices.
- Separating tunnel validation from gateway configuration simplifies troubleshooting and reduces the number of simultaneous network changes.
