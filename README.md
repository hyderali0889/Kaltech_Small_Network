
# Kaltech Small Network Project

## Overview

The Kaltech Small Network Project provides a comprehensive, scalable, and secure network design for a small-to-medium-sized organization. This project includes features such as VLANs, subnets, DHCP clients and relays, routing, security, and more to ensure that the network operates efficiently and reliably.

## Table of Contents
1. [Introduction](#introduction)
2. [Network Design](#network-design)
    - [VLANs](#vlans)
    - [Subnets](#subnets)
    - [DHCP](#dhcp)
    - [Routing](#routing)
    - [Security](#security)
3. [Implementation Guide](#implementation-guide)
    - [Devices and Equipment](#devices-and-equipment)
    - [IP Addressing Plan](#ip-addressing-plan)
    - [Network Configuration](#network-configuration)
4. [Troubleshooting and Maintenance](#troubleshooting-and-maintenance)
5. [Conclusion](#conclusion)

---

## Introduction

The Kaltech Small Network Project is designed to meet the needs of small offices, businesses, or branch locations by providing a robust, secure, and efficient network. This design leverages VLAN segmentation, IP subnetting, and DHCP configurations to support network scalability and security.

## Network Design

### VLANs
Virtual LANs (VLANs) segment the network into smaller, more manageable broadcast domains. This helps reduce broadcast traffic, improves security, and simplifies network management.

#### VLAN Configuration:
- **VLAN 10**: Management Network
- **VLAN 20**: Sales Network
- **VLAN 30**: HR Network
- **VLAN 40**: Guest Network
- **VLAN 50**: Server Network

#### VLAN Configuration Example:
```bash
vlan 10
 name Management
vlan 20
 name Sales
vlan 30
 name HR
vlan 40
 name Guest
vlan 50
 name Server
```

### Subnets
Each VLAN is assigned a unique subnet to optimize IP addressing and ensure efficient routing.


| 10   | 192.168.10.0   | 255.255.255.0    |

| 20   | 192.168.20.0   | 255.255.255.0    |

| 30   | 192.168.30.0   | 255.255.255.0    |

| 40   | 192.168.40.0   | 255.255.255.0    |

| 60   | 192.168.60.0   | 255.255.255.0    |

### DHCP
Dynamic Host Configuration Protocol (DHCP) allows for automatic IP address allocation within each subnet. The DHCP server is configured to assign IP addresses dynamically to clients within the designated IP pool.

#### DHCP Scope Configuration:
```bash
ip dhcp pool Sales
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8

ip dhcp pool HR
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 8.8.8.8
```

### DHCP Relay
A DHCP relay agent is configured to forward DHCP requests between clients in different VLANs and the central DHCP server.

#### DHCP Relay Configuration:
```bash
ip helper-address 192.168.10.2   # IP address of the DHCP server
```

### Routing
Routing ensures that devices in different subnets can communicate with each other. A router or Layer 3 switch is used to route traffic between VLANs.

#### Routing Configuration Example:
```bash
ip routing
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown

interface vlan 30
 ip address 192.168.30.1 255.255.255.0
 no shutdown

interface vlan 40
 ip address 192.168.40.1 255.255.255.0
 no shutdown

interface vlan 50
 ip address 192.168.50.1 255.255.255.0
 no shutdown
```

### Security
Security measures are implemented to protect the network from unauthorized access, malicious traffic, and potential vulnerabilities.

- **ACLs (Access Control Lists)** are used to restrict access between VLANs.
- **Port Security** is enabled on switches to restrict the number of MAC addresses per port.
- **802.1X Authentication** is used for wired access control.

#### ACL Example:
```bash
access-list 100 permit ip 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255
access-list 100 deny ip any any
```

#### Port Security Example:
```bash
switchport port-security
switchport port-security maximum 2
switchport port-security violation restrict
```

## Implementation Guide

### Devices and Equipment
- **Router**: Cisco ISR 4000 or equivalent
- **Layer 3 Switch**: Cisco Catalyst 3000 Series
- **Access Switches**: Cisco Catalyst 2960
- **Firewall**: Palo Alto Networks PA-Series (optional)
- **Wireless Access Points**: Aruba or Cisco Meraki

### IP Addressing Plan
A structured IP addressing plan is essential for network scalability. Below is a sample addressing scheme:


| Management PC   | 192.168.10.10    | 255.255.255.0    | 192.168.10.1    |

| Sales PC        | 192.168.20.10    | 255.255.255.0    | 192.168.20.1    |

| HR PC           | 192.168.30.10    | 255.255.255.0    | 192.168.30.1    |

| Guest Laptop    | 192.168.40.10    | 255.255.255.0    | 192.168.40.1    |

| Server          | 192.168.50.10    | 255.255.255.0    | 192.168.50.1    |

### Network Configuration
1. **Configure Switch VLANs and Trunking**: 
   - Set up VLANs on all switches.
   - Enable trunking on the ports connecting switches.

2. **Router Configuration**:
   - Enable routing and configure the sub-interfaces for each VLAN.
   - Configure inter-VLAN routing.

3. **Configure DHCP Server**:
   - Set up DHCP pools for each VLAN.
   - Enable the DHCP relay agent on routers.

4. **Security Configuration**:
   - Configure ACLs to control inter-VLAN traffic.
   - Enable port security on switch ports.

5. **Test and Verify**:
   - Test connectivity between devices in different VLANs.
   - Verify DHCP functionality.
   - Test security features such as ACLs and port security.

## Troubleshooting and Maintenance

### Common Issues:
1. **DHCP Not Assigning IP Addresses**:
   - Verify the DHCP server is reachable.
   - Ensure the DHCP relay agent is configured correctly.

2. **VLAN Communication Issues**:
   - Check that VLANs are properly assigned on both the switch and router.
   - Verify the routing table is correct.

3. **Slow Network Performance**:
   - Check for network loops and ensure Spanning Tree Protocol (STP) is enabled.

4. **Unauthorized Access**:
   - Review ACLs and port security settings.
   - Ensure proper VLAN assignments are made.

### Maintenance Tasks:
- Regularly update network devices (routers, switches, firewalls) with the latest firmware.
- Perform periodic audits of security configurations (e.g., ACLs, port security, 802.1X).
- Backup configuration files for all network devices.

## Conclusion

The Kaltech Small Network Project is designed to provide a secure, scalable, and efficient network for small to medium-sized organizations. With a clear focus on VLAN segmentation, IP addressing, DHCP management, and network security, this network architecture ensures smooth operations and room for future growth.
```
