# Network Configuration

## Current Configuration

| Item | Value |
|------|------|
| Physical Topology | Internet → ONT → Hills Home Hub → eero 6 → Intel NUC |
| Connection Type | Wi-Fi (wlp0s20f3) |
| IP Assignment | DHCP |
| Default Gateway | eero 6 (192.168.4.1) |
| DNS Servers | 1.1.1.1, 8.8.8.8 |
| Internet Access | Operational |

---

## Validation

The initial network configuration was validated before deploying additional infrastructure services.

The validation confirmed:

- The server successfully obtained an IP address via DHCP.
- The default gateway is reachable.
- The routing table was configured correctly.
- DNS name resolution is working.
- Internet connectivity was verified.
- The public IP address was recorded before the VPN deployment.
