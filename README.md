# OPNsense VLAN Segmentation Lab

A hands-on cybersecurity lab built using Hyper-V and OPNsense to simulate network segmentation using VLANs.

This lab demonstrates how to separate network traffic between a server and a client using VLANs, configure routing and firewall rules, and verify isolation in a virtual environment.

---

## Lab Overview

This lab simulates a small enterprise network where devices are placed in separate VLANs for security and isolation.

Components:

- Hyper-V virtualization  
- OPNsense firewall/router  
- Windows Server (Server VLAN)  
- Windows 11 (Client VLAN)  

---

## Network Topology

                    INTERNET
                        |
                   [ WAN - hn0 ]
                        |
                 +----------------+
                 |    OPNsense    |
                 |   Firewall     |
                 +--------+-------+
                          |
                    [ LAN - hn1 ]
                          |
                 (Trunk VLAN 10,20)
                          |
                  +----------------+
                  |  LAN-Switch    |
                  +--------+-------+
                           | 
             +-------------+-------------+
             |                           |
     VLAN 10 (Server)            VLAN 20 (Client)
     192.168.30.0/24             192.168.40.0/24
             |                           |
     Windows Server               Windows 11
   192.168.30.145               192.168.40.144
   GW: 192.168.30.1             GW: 192.168.40.1

---

## Network Design

| Network        | VLAN ID | Subnet            | Gateway        | Purpose              |
|----------------|--------|------------------|----------------|----------------------|
| LAN (Mgmt)     | -      | 192.168.10.0/24  | 192.168.10.1   | Management network   |
| Server VLAN    | 10     | 192.168.30.0/24  | 192.168.30.1   | Secure server zone   |
| Client VLAN    | 20     | 192.168.40.0/24  | 192.168.40.1   | User/client network  |

---

## Key Features

- VLAN configuration in OPNsense  
- Trunk configuration in Hyper-V  
- DHCP per VLAN using Dnsmasq  
- Firewall rules for traffic control  
- Network segmentation and isolation  

---

## Security Concept

This lab demonstrates network segmentation as a core security principle.

- The server is isolated in a dedicated VLAN  
- The client network is separated from the server network  
- Firewall rules explicitly block traffic between VLANs  
- Both VLANs are allowed to access the internet  

This reduces lateral movement and improves internal network security.

---

## Verification

The configuration was tested and verified:

| Test                          | Result              |
|-------------------------------|---------------------|
| Server IP assignment          | 192.168.30.145      |
| Client IP assignment          | 192.168.40.144      |
| Ping Server → Client          | Failed (Blocked)    |
| Ping Client → Server          | Failed (Blocked)    |
| Internet access (both VLANs)  | Success             |

---

## Screenshots

All screenshots are stored in:

docs/images/

Included:

- VLAN configuration  
- Interface setup  
- DHCP configuration (Dnsmasq)  
- Firewall rules (block + allow)  
- IP configuration (Server & Client)  
- Connectivity test results  

---

## Documentation

Detailed step-by-step guide is available in:

docs/lab-documentation.md

Includes:

- Full configuration steps  
- Hyper-V VLAN setup  
- OPNsense configuration  
- Troubleshooting  
- Validation and testing  

---

## What I Learned

- How VLANs segment network traffic  
- How to configure VLANs in OPNsense  
- How Hyper-V handles VLAN tagging and trunking  
- How firewall rules control inter-network traffic  
- How to troubleshoot real-world lab issues (RAM, DHCP, VLAN)  

---

## Future Improvements

- Allow specific traffic between VLANs (e.g. RDP or ICMP)  
- Enable firewall logging  
- Integrate IDS/IPS (Suricata)  
- Expand lab with additional VLANs and services  

---

## Author

Muhammad Mehdi  
IT Security Developer Student
