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
