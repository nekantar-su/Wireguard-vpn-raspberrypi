# Wireguard-vpn-raspberrypi
Secure WireGuard VPN server deployed on a Raspberry Pi, featuring port forwarding, NAT masquerading, and firewall rules.
# WireGuard VPN on Raspberry Pi

## Overview
This project sets up a **WireGuard VPN server** on a Raspberry Pi for secure remote access. It includes firewall rules, port forwarding, and logging to ensure stable and secure connections.

### Features:
- Full **WireGuard VPN setup** for secure remote access
- Configured **firewall rules (iptables, NAT, and port forwarding)**
- **DHCP troubleshooting and NAT masquerading** for proper routing
- **Systemd service for auto-start on reboot**
- **Basic logging** to track client connections

## Setup Instructions

### 1.Install WireGuard
```bash
sudo apt update && sudo apt install -y wireguard

### 2. Generate Keys
wg genkey | tee server_private.key | wg pubkey > server_public.key
wg genkey | tee client_private.key | wg pubkey > client_public.key

### 3. Configure Wireguard Server
[Interface]
PrivateKey = <server_private_key>
Address = 10.0.0.1/24
ListenPort = 51820
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT

[Peer]
PublicKey = <client_public_key>
AllowedIPs = 10.0.0.2/32

### 4.Start Wireguard
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0

###5.Configure Client

[Interface]
PrivateKey = <client_private_key>
Address = 10.0.0.2/24
DNS = 8.8.8.8

[Peer]
PublicKey = <server_public_key>
Endpoint = <your-public-ip>:51820
AllowedIPs = 0.0.0.0/0

### 6.Troubleshooting
#Network unreachable error:
sudo dhclient -r && sudo dhclient
#Check VPN status
sudo systemctl status wg-quick@wg0

###SCREENSHOTS
#Wireguard Status
1.Shows wireguard server running on Raspberry Pi
![Wireguard Status] (screenshots/wg.status.png)

2.Windows Client running Wireguard
![Wireguard Windows] (screenshot/wireguard-windows.png)
Built by: Nicholas Kantarellis
