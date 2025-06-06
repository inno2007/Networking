# IPv4 Routing Concepts & Static Routes

This document summarizes routing theory essential to networking and cybersecurity. It includes core routing operations, static routing types, route selection logic, and command references.

---

## 1. What Is Routing?

Routing is the process by which routers determine the best path for forwarding packets between different networks.

### Routers perform:
- **Routing decision**: Based on destination IP and routing table
- **Encapsulation**: Re-wraps packets into new data link frames
- **Forwarding**: Sends out the packet to the next-hop router

---

## 2. Directly Connected vs Remote Routes

- **Directly Connected**: Learned automatically when an interface is configured and active  
- **Remote**: Learned via static configuration or dynamic routing protocols

To view:
```
show ip route
```

| Code | Meaning          |
|------|------------------|
| C    | Connected        |
| L    | Local            |
| S    | Static           |
| D    | EIGRP            |
| O    | OSPF             |

---

## 3. Static Routing

### Benefits
- Secure (no updates from external routers)
- Low resource usage (CPU, bandwidth)
- Ideal for small networks and stub connections

### Use Cases
- Routing to a single destination
- Stub networks (e.g., LAN with one router)
- Backup (floating) routes
- Summary routes

---

## 4. Static Route Configuration

### Basic Syntax
```
ip route <destination-network> <subnet-mask> <next-hop-IP or exit-interface>
```

### Examples

#### 1. Recursive Static Route (Next-hop IP)
```
ip route 192.168.1.0 255.255.255.0 10.1.1.2
```

#### 2. Direct Static Route (Exit interface only)
```
ip route 192.168.3.0 255.255.255.0 s0/0/0
```

#### 3. Fully Specified Route (Both next-hop and interface)
```
ip route 192.168.2.0 255.255.255.0 g0/1 192.168.1.2
```

---

## 5. Default Routes

- Used as a “catch-all” for unknown destinations  
- Useful at the edge of a network  
- Identified by destination `0.0.0.0 0.0.0.0`

```
ip route 0.0.0.0 0.0.0.0 s0/0/0
```

---

## 6. Floating Static Routes (Backup Routes)

- Only used if the primary route fails
- Have higher Administrative Distance (AD) than primary route

```
ip route 192.168.2.0 255.255.255.0 10.1.1.2 91
```

*(If EIGRP is AD 90, this backup route activates only when EIGRP fails)*

---

## 7. Best Path Selection

Routers select paths using **metrics**:

| Protocol | Metric Used         |
|----------|---------------------|
| RIP      | Hop count           |
| OSPF     | Cost (bandwidth)    |
| EIGRP    | Bandwidth, delay    |

If multiple paths have identical metrics, the router uses **equal-cost load balancing**.

---

## 8. Administrative Distance (AD)

When multiple protocols advertise the same destination, AD is used to choose the most reliable source.

| Source        | AD Value |
|---------------|----------|
| Connected     | 0        |
| Static        | 1        |
| EIGRP         | 90       |
| OSPF          | 110      |
| RIP           | 120      |

Lower AD = More trusted

---

## 9. Common Troubleshooting Commands

| Command                              | Purpose                             |
|--------------------------------------|-------------------------------------|
| `show ip route`                      | View routing table                  |
| `show ip route static`              | Show static routes                  |
| `show ip interface brief`           | Interface status and IP             |
| `ping <destination-IP>`             | Test connectivity                   |
| `traceroute <destination-IP>`       | Track route hops                    |

---

## 10. Summary

Routing ensures communication between different subnets/networks. Static routes give full control but require manual updates. Understanding how routes are selected and verified helps in both securing and optimizing a network.

