# Switching, VLANs, and Port Security – Theory Overview

This document explains Layer 2 switching fundamentals, VLAN configurations, trunking protocols, and port security. These concepts support real-world Cisco switch configuration and network segmentation best practices.

---

## 1. Layer 2 Switching Concepts

- **Switching** is the process of forwarding frames based on **MAC addresses**.
- Operates at **Layer 2 (Data Link)** of the OSI model.
- Switches maintain a **MAC address table** to associate ports with device addresses.
- Switches **do not segment broadcast domains** — all ports share the same domain unless VLANs are configured.

---

## 2. VLAN (Virtual LAN) Basics

- A VLAN is a **logical group of devices** on a LAN, separated at Layer 2.
- Each VLAN creates a **separate broadcast domain**.
- VLANs allow network segmentation by **role**, not just location.

### Benefits of VLANs:
- Limits broadcast traffic
- Improves security and performance
- Easier administration and scaling

### Common VLAN Types:
| VLAN Type       | Purpose                                  |
|------------------|------------------------------------------|
| Default VLAN     | VLAN 1 – assigned by default             |
| Data VLAN        | Carries user data (e.g., internet, email)|
| Native VLAN      | Handles untagged traffic on trunks       |
| Management VLAN  | Used for remote switch access (e.g., SSH)|

---

## 3. VLAN Configuration (Cisco IOS)

```bash
vlan 10
 name Students
vlan 20
 name Faculty
```

### Assign Ports to VLANs
```bash
interface f0/6
 switchport mode access
 switchport access vlan 10

interface f0/18
 switchport mode access
 switchport access vlan 20
```

---

## 4. VLAN Trunking

Trunk ports carry traffic for **multiple VLANs** using **802.1Q tagging**.

```bash
interface f0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,99
```

### Tagging:
- Frames on trunk links include a VLAN tag (VID) via 802.1Q
- Native VLAN traffic is **not tagged**

---

## 5. Inter-VLAN Routing (Router-on-a-Stick)

Used when hosts in different VLANs need to communicate.

### Router Subinterface Setup:
```bash
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface g0/0.99
 encapsulation dot1Q 99 native
 ip address 192.168.99.1 255.255.255.0
```

---

## 6. Switch Management (SVI)

SVI (Switched Virtual Interface) enables switch management:

```bash
interface vlan 99
 ip address 192.168.1.2 255.255.255.0
 no shutdown

ip default-gateway 192.168.1.1
```

Assign all ports to the management VLAN:

```bash
interface range f0/1 – 24, g0/1 – 2
 switchport mode access
 switchport access vlan 99
```

---

## 7. Port Security

Used to prevent unauthorized devices from connecting to the switch.

### Basic Configuration:
```bash
interface f0/5
 switchport mode access
 switchport port-security
 switchport port-security mac-address <MAC>
```

### Enable Sticky MAC:
```bash
switchport port-security mac-address sticky
```

### View Security Settings:
```bash
show port-security
show port-security interface f0/5
```

### Reset Port After Violation:
```bash
interface f0/5
 shutdown
 no shutdown
```

---

## 8. MAC Address Table Management

### Show & Clear Commands:
```bash
show mac address-table
clear mac address-table dynamic
```

### Add Static MAC Address:
```bash
mac address-table static <MAC> vlan 99 interface f0/6
```

---

## 9. VLAN Verification & Troubleshooting

| Command                        | Description                          |
|--------------------------------|--------------------------------------|
| `show vlan brief`             | View all VLANs and port assignments  |
| `show interfaces trunk`       | Check trunk status                   |
| `show interfaces f0/1 switchport` | VLAN mode, native VLAN, etc.      |
| `show ip route`               | Confirm routing between VLANs        |

---

## 10. Summary

Switching and VLANs form the foundation of secure, segmented networks. Using VLANs with proper trunking, port security, and inter-VLAN routing enables scalable, manageable enterprise environments.

