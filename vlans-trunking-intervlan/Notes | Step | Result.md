# VLANs, Trunking, and Inter-VLAN Routing

## Objective  
Segment a switched network using VLANs, enable communication between VLANs using a router or Layer 3 switch, and configure trunk links for carrying multiple VLANs.

---

## Key Concepts

- VLAN (Virtual LAN) separates devices at Layer 2 without physical separation  
- Trunk links allow multiple VLANs to share one interface between switches  
- Inter-VLAN routing enables communication between VLANs  
- Two methods:
  - Router-on-a-Stick (Router + Subinterfaces)
  - Layer 3 Switch (SVI)

---

## Example Topology

- PC0 and PC1 in VLAN 10  
- PC2 and PC3 in VLAN 20  
- Switch1 connected to Router via trunk  
- Router provides routing between VLANs

---

## VLAN Configuration (Switch)

1. Create VLANs:
```
vlan 10
name STAFF
exit
vlan 20
name STUDENT
```

2. Assign VLANs to interfaces:
```
interface fa0/1
switchport mode access
switchport access vlan 10
exit

interface fa0/2
switchport mode access
switchport access vlan 20
```

---

## Configure Trunk Port (Switch to Router or Switch)

```
interface g0/1
switchport trunk encapsulation dot1q
switchport mode trunk
```

---

## Router-on-a-Stick Configuration (Router)

1. Create subinterfaces:
```
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

2. Enable the main interface:
```
interface g0/0
no shutdown
```

---

## Configure PCs

- PC0: 192.168.10.10 /24, gateway: 192.168.10.1  
- PC2: 192.168.20.10 /24, gateway: 192.168.20.1

---

## Inter-VLAN Routing Using Layer 3 Switch (Alternative)

1. Enable routing:
```
ip routing
```

2. Create SVI interfaces:
```
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown

interface vlan 20
ip address 192.168.20.1 255.255.255.0
no shutdown
```

3. Assign switchports to VLANs as usual.

---

## Verifying Configuration

Use:
```
show vlan brief
show interfaces trunk
show ip route
ping <IP of device in another VLAN>
```

---

## Notes

- Use `dot1Q` encapsulation for trunking  
- Each VLAN needs its own subnet and default gateway  
- Inter-VLAN routing requires Layer 3 awareness  
- If pings fail: check trunk, VLAN assignment, subinterfaces
