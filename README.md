# OPNsense VLAN Segmentation Lab

A hands-on cybersecurity lab built using **Hyper-V** and **OPNsense** to simulate network segmentation using VLANs.

This lab demonstrates how to separate network traffic between a server and a client using VLANs, configure routing and firewall rules, and verify isolation in a virtual environment.

---

## Lab Overview

This lab simulates a small enterprise network where different devices are placed in separate VLANs for security and isolation.

### Components:

- Hyper-V virtualization
- OPNsense firewall/router
- Windows Server (Server VLAN)
- Windows 11 (Client VLAN)

---

## Network Design

- LAN (Management Network): `192.168.10.0/24`
- VLAN 10 (Server): `192.168.30.0/24`
- VLAN 20 (Client): `192.168.40.0/24`

Each VLAN is configured on OPNsense and connected through a trunk interface in Hyper-V.

---

## Key Features

- VLAN configuration in OPNsense
- Trunk configuration in Hyper-V
- DHCP per VLAN using Dnsmasq
- Firewall rules for traffic control
- Network segmentation and isolation

---

## Security Concept

The purpose of this lab is to demonstrate **network segmentation**.

- The server is placed in a separate VLAN to protect sensitive resources
- The client is isolated from the server network
- Firewall rules block traffic between VLANs
- Both networks still have access to the internet

This setup reduces the attack surface and improves internal network security.

---

## Verification

The configuration was tested to verify correct behavior:

- Server receives IP: `192.168.30.x`
- Client receives IP: `192.168.40.x`
- Ping between VLANs fails (blocked by firewall)
- Internet access works from both networks

---

## Screenshots

All screenshots are available in the `docs/images` folder.

- VLAN configuration
- Interface setup
- DHCP configuration
- Firewall rules
- IP configuration (Server & Client)
- Connectivity tests

---

## What I Learned

- How VLANs segment network traffic
- How to configure VLANs in OPNsense
- How Hyper-V handles VLAN tagging and trunking
- How firewall rules control traffic between networks
- How to troubleshoot real-world lab issues (RAM limitations, VLAN configuration, DHCP)

---

## Future Improvements

- Allow specific traffic between VLANs (e.g. RDP, ICMP)
- Add logging to firewall rules
- Integrate IDS/IPS for traffic monitoring
- Expand lab with additional VLANs and services

---

## Author

Muhammad Mehdi  
IT Security Developer Student
