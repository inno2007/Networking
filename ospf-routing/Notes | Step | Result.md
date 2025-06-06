# OSPF Routing – Dynamic Route Exchange

## Objective  
Configure and verify OSPF (Open Shortest Path First) routing between multiple routers. Learn to share routes dynamically without static configuration and optimize route propagation.

---

## Key Concepts

- OSPF is a link-state dynamic routing protocol  
- It uses cost as the metric (based on bandwidth)  
- Routers form adjacencies and share route information  
- OSPF Area 0 is the backbone area

---

## Topology Overview

Example:
R1 – R2 – R3

- R1 LAN: 192.168.1.0/24  
- R2 LAN: 192.168.2.0/24  
- R3 LAN: 192.168.3.0/24  
- R1 ↔ R2: 10.0.0.0/30  
- R2 ↔ R3: 10.0.0.4/30

---

## Configuration Steps

### Step 1: Enable OSPF on All Routers

Enter router config mode:
```
conf t
router ospf 1
```

Use `network` command to advertise each LAN and link:
```
network 192.168.1.0 0.0.0.255 area 0
network 10.0.0.0 0.0.0.3 area 0
```

Repeat for each router with its own networks.

---

## Verifying OSPF

Use these commands:
```
show ip route
show ip ospf neighbor
show ip protocols
```

- Look for OSPF routes marked with `O`
- Check OSPF neighbors are in FULL state

---

## Passive Interfaces 

To prevent OSPF Hello packets on LANs:
```
passive-interface g0/0
```

Use only on LANs that don’t need OSPF neighbors.

---

## Load Balancing and Cost 

Change interface cost manually:
```
interface g0/1
ip ospf cost 10
```

OSPF uses lower cost to prefer a route.

---

## Cisco Packet Tracer Used

- Use 3 routers, 3 switches, 3 PCs  
- Assign IPs and enable OSPF  
- Test by pinging between all PCs  
- Use `show` commands to check OSPF status

---

## Notes

- OSPF is scalable and converges fast  
- Always start with Area 0 for backbone  
- Subnet wildcard mask = inverse of subnet mask  
- Check for mismatched Hello/Dead timers if neighbors fail to form
