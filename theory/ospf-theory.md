# OSPF (Open Shortest Path First) – Routing Theory & Commands

This document summarizes the essential concepts and commands for configuring and understanding OSPFv2 in IPv4 networks. OSPF is a link-state routing protocol used for dynamic path determination in medium to large networks.

---

## 1. What Is OSPF?

- **OSPF (Open Shortest Path First)** is a dynamic routing protocol based on **link-state** logic.
- It calculates the best path using **Dijkstra’s SPF (Shortest Path First)** algorithm.
- Each router maintains a **Link-State Database (LSDB)** and independently builds a **routing table**.

---

## 2. Key Features of OSPF

- Classless protocol (supports VLSM and CIDR)
- Fast convergence through link-state advertisements
- Metric = **Cost**, based on interface **bandwidth**
- Supports **multi-area design** for scalability
- Uses **Hello packets** to maintain neighbor relationships
- **Administrative Distance** = 110

---

## 3. OSPF Packet Types

| Packet Type       | Purpose                              |
|-------------------|--------------------------------------|
| Hello             | Discover and maintain neighbors      |
| Database Description (DBD) | Summarize LSDB               |
| Link-State Request (LSR) | Request specific LSAs         |
| Link-State Update (LSU)  | Send LSAs                      |
| Link-State Acknowledgment (LSAck) | Confirm receipt       |

---

## 4. Basic OSPF Configuration

```bash
router ospf 1
 router-id 1.1.1.1
 network 192.168.1.0 0.0.0.255 area 0
```

- **router ospf 1**: Enables OSPF with process ID `1`
- **router-id**: Manually sets the router's identity
- **network + wildcard + area**: Advertises the specified network in Area 0

---

## 5. Wildcard Masks

- Inverse of subnet masks  
  Example:  
  Subnet Mask `255.255.255.0` → Wildcard `0.0.0.255`

---

## 6. DR and BDR Elections (Multi-access networks)

| Role | Description |
|------|-------------|
| DR   | Designated Router: central point for LSDB exchange |
| BDR  | Backup Designated Router: standby to DR |
| Others | Form adjacencies only with DR and BDR |

- Use OSPF **priority** to influence election
```bash
interface g0/0
 ip ospf priority 100
```

---

## 7. Adjusting OSPF Behavior

### Set OSPF Cost (override default bandwidth-based cost)
```bash
interface g0/0
 ip ospf cost 10
```

### Passive Interface (disable Hello packets on LAN)
```bash
router ospf 1
 passive-interface g0/1
```

### Hello and Dead Intervals
```bash
interface g0/0
 ip ospf hello-interval 10
 ip ospf dead-interval 40
```

### Reference Bandwidth (for accurate cost scaling)
```bash
router ospf 1
 auto-cost reference-bandwidth 1000
```

---

## 8. Default Route Redistribution into OSPF

```bash
ip route 0.0.0.0 0.0.0.0 <next-hop>
router ospf 1
 default-information originate
```

---

## 9. OSPF Authentication (MD5)

```bash
interface g0/0
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 Cisco123
```

---

## 10. Verification Commands

| Command | Purpose |
|---------|---------|
| `show ip ospf` | OSPF process info |
| `show ip ospf neighbor` | List neighbors |
| `show ip ospf interface` | View timers, cost, DR/BDR info |
| `show ip route ospf` | View learned OSPF routes |
| `show ip ospf database` | View LSDB |

---

## 11. Alternative: OSPF on Interface Directly

Instead of using `network` statements:

```bash
interface g0/0
 ip ospf 1 area 0
```

---

## Summary

OSPF is a robust and scalable routing protocol. It's ideal for large, hierarchical enterprise networks and provides rapid convergence through SPF and LSA mechanisms. Understanding the OSPF cost model, router roles, and command structure is critical for managing dynamic routing securely and efficiently.

