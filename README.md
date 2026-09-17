# 🏢 Small Office Network Implementation

A complete small office network designed and configured in **Cisco Packet Tracer** for a 15-employee company.

## 📌 Project Overview

| Item | Detail |
|------|--------|
| **Project** | Small Office Network Implementation |
| **Tool** | Cisco Packet Tracer |
| **Difficulty** | Beginner → Intermediate |
| **Network** | IPv4 — 192.168.10.0/24 |

---

## 🎯 Requirements

- 1 Cisco Router (default gateway)
- 1 Cisco Switch
- 15 PCs (DHCP-enabled)
- 1 Server (static IP)
- All PCs receive IP automatically via DHCP
- PCs can communicate with each other and the Server

---

## 📊 IP Addressing Plan

| Device | Interface | IP Address | Mask | Assignment |
|--------|-----------|------------|------|------------|
| Router | G0/0/0 | 192.168.10.1 | /24 | Static |
| Switch | VLAN 1 | 192.168.10.2 | /24 | Static |
| Server | NIC | 192.168.10.10 | /24 | Static |
| PCs 1–15 | NIC | 192.168.10.100–114 | /24 | DHCP |

**DHCP Pool:** 192.168.10.100 – 192.168.10.200  
**Excluded:** 192.168.10.1 – 192.168.10.20

---

## ⚙️ Device Configurations

### Router (ISR 4331)

```cisco
enable
configure terminal
hostname Office-Router

interface GigabitEthernet0/0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

ip dhcp excluded-address 192.168.10.1 192.168.10.20

ip dhcp pool OFFICE-LAN
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.10.10
 exit

end
write memory
#Switch Configuration
enable
configure terminal
hostname Office-Switch

interface vlan 1
 ip address 192.168.10.2 255.255.255.0
 no shutdown
 exit
ip default-gateway 192.168.10.1

interface range FastEthernet0/1 - 16
 switchport mode access
 no shutdown
 exit

end
write memory
#Server Configuration
Server Configuration
IP Address: 192.168.10.10

Subnet Mask: 255.255.255.0

Default Gateway: 192.168.10.1

DNS Server: 192.168.10.10
#Test Results
#Test 1 - PC receives IP automatically
C:\> ipconfig
IPv4 Address................: 192.168.10.100
Subnet Mask.................: 255.255.255.0
Default Gateway.............: 192.168.10.1
DNS Server..................: 192.168.10.10
PASS
#Test 2 - Ping Default Gateway
C:\> ping 192.168.10.1
Reply from 192.168.10.1: bytes=32 time<1ms TTL=255 (x4)
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
PASS
#Test 3 - Ping Server
C:\> ping 192.168.10.10
Reply from 192.168.10.10: bytes=32 time<1ms TTL=128 (x4)
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
PASS
#Test 4 - PC-to-PC Communication
C:\> ping 192.168.10.101
Reply from 192.168.10.101: bytes=32 time<1ms TTL=128 (x4)
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
PASS
