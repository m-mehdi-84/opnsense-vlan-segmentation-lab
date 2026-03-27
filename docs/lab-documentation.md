# lab-documentation.md

## Objective

The goal of this lab was to configure VLAN segmentation in OPNsense inside a Hyper-V environment and verify that two different systems could be placed in separate VLANs with controlled traffic between them.

The final result was:

- Windows Server placed in VLAN 10
- Windows 11 placed in VLAN 20
- DHCP working on both VLANs
- Traffic between VLANs blocked
- Internet access allowed from both VLANs

---

## Environment

### Virtual Machines

- OPNsense
- Windows Server
- Windows 11

### OPNsense Interfaces

- WAN: hn0
- LAN: hn1

### Existing Setup

An existing LAN was already configured and in use before this lab:

- Network: 192.168.10.0/24
- OPNsense LAN IP: 192.168.10.1
- DHCP already enabled

To avoid breaking the environment, the existing LAN was kept unchanged and VLANs were added on top of the LAN interface.

---

## VLAN Plan

| VLAN Name   | VLAN ID | Subnet           | Gateway       |
|-------------|---------|------------------|---------------|
| Server_VLAN | 10      | 192.168.30.0/24  | 192.168.30.1  |
| Client_VLAN | 20      | 192.168.40.0/24  | 192.168.40.1  |

---

## Step 1: Create VLANs in OPNsense

Navigate to the VLAN section in OPNsense (menu may vary depending on version).

Create the following VLANs on parent interface hn1:

VLAN 10
- Parent Interface: hn1
- VLAN Tag: 10
- Description: Server_VLAN

VLAN 20
- Parent Interface: hn1
- VLAN Tag: 20
- Description: Client_VLAN

---

## Step 2: Assign VLAN Interfaces

Go to Interfaces → Assignments and add both VLANs.

Rename and configure:

Server_VLAN
- Enable interface
- IPv4 Address: 192.168.30.1/24

Client_VLAN
- Enable interface
- IPv4 Address: 192.168.40.1/24

---

## Step 3: Configure DHCP (Dnsmasq)

In the current OPNsense version, DHCP was configured using Dnsmasq DNS & DHCP.

Configure:

Server_VLAN
- Start: 192.168.30.100
- End: 192.168.30.200

Client_VLAN
- Start: 192.168.40.100
- End: 192.168.40.200

Ensure Dnsmasq is enabled and both VLAN interfaces are selected.

---

## Step 4: Configure VLAN in Hyper-V

Windows Server
- Enable VLAN identification
- VLAN ID: 10

Windows 11
- Enable VLAN identification
- VLAN ID: 20

---

## Step 5: Configure Trunk (OPNsense LAN)

The OPNsense LAN adapter connected to LAN-Switch was configured as a trunk.

PowerShell command used:

Get-VMNetworkAdapter -VMName "OPNsense-VM" | Where-Object { $_.SwitchName -eq "LAN-Switch" } | Set-VMNetworkAdapterVlan -Trunk -AllowedVlanIdList "10,20" -NativeVlanId 1

Verification command:

Get-VMNetworkAdapter -VMName "OPNsense-VM" | Where-Object { $_.SwitchName -eq "LAN-Switch" } | Get-VMNetworkAdapterVlan

Result:
- Mode: Trunk
- VLAN List: 1,10,20

---

## Step 6: Configure Firewall Rules

Server_VLAN
1. Block → Destination: Client_VLAN net
2. Pass → Destination: any

Client_VLAN
1. Block → Destination: Server_VLAN net
2. Pass → Destination: any

Important:
- Rules are processed top-down
- Block rules must be above pass rules

---

## Step 7: Verify IP Assignment

Windows Server
- IP: 192.168.30.145
- Gateway: 192.168.30.1

Windows 11
- IP: 192.168.40.144
- Gateway: 192.168.40.1

This confirms DHCP and VLAN configuration are working.

---

## Step 8: Test Connectivity

Inter-VLAN Test

From Windows Server:
ping 192.168.40.144

From Windows 11:
ping 192.168.30.145

Result:
- Both tests failed (expected)

Internet Test

From both systems:
ping 8.8.8.8

Result:
- Success on both systems

---

## Problems Encountered and Fixes

1. DHCP configuration in OPNsense  
The DHCP setup differs in the current OPNsense version, where Dnsmasq is used instead of the legacy DHCPv4 interface.

Fix:
- Configured DHCP using Dnsmasq DNS & DHCP for both VLAN interfaces

2. Existing LAN already in use  
The original network was active.

Fix:
- Kept LAN unchanged
- Created new VLAN subnets

3. Hyper-V adapter naming confusion  
All adapters had the same name.

Fix:
- Identified correct adapter using SwitchName

4. RAM allocation issues  
VMs could not start due to insufficient memory.

Fix:
- Reduced RAM
- Started only required VMs

5. Firewall rule order  
Incorrect rule order would allow traffic.

Fix:
- Placed block rules above pass rules

---

## Final Result

The lab was successfully completed with the following outcome:

- Server and client are placed in separate VLANs
- Traffic between VLANs is blocked
- Both VLANs have internet access
- Existing LAN environment remains unaffected

This demonstrates practical network segmentation using VLANs in a virtualized environment.
