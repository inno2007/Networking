# DHCPv4 – Automatic IP Address Assignment and Relay Configuration

## Objective  
Configure a Cisco router to automatically assign IP addresses to hosts using DHCP. Learn to create DHCP address pools, exclude reserved addresses, and set up a DHCP relay if the server is not in the same network.

---

## Key Concepts

- **DHCP (Dynamic Host Configuration Protocol)** assigns:
  - IP Address
  - Subnet Mask
  - Default Gateway
  - DNS Server
- **DHCP Excluded Addresses** prevent specific IPs (e.g., gateways or servers) from being assigned
- **DHCP Relay Agent (ip helper-address)** allows clients to receive IPs from a server on another subnet

---

## DHCP Server Configuration on a Router

### Step 1: Exclude Reserved IPs

```
ip dhcp excluded-address 192.168.1.1 192.168.1.10
```

### Step 2: Create DHCP Pool

```
ip dhcp pool STAFF
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
```

---

## Verifying DHCP Functionality

On client PCs in Packet Tracer:
- Set IP configuration to DHCP
- Click "DHCP" → Receive IP from router

On router:
```
show ip dhcp binding
show running-config | section dhcp
```

---

## DHCP Relay Agent Configuration (If DHCP server is on another subnet)

### On client-facing router interface:

```
interface g0/1
ip helper-address 192.168.2.100
```

- `192.168.2.100` is the IP of the DHCP server

---

## Example Topology

- Router R1 connected to LAN (192.168.1.0/24)  
- PCs assigned IPs from pool `STAFF`  
- DHCP server optionally on another router (Relay scenario)

---

## Notes

- Always exclude router and reserved IPs  
- Each subnet should have a unique DHCP pool  
- DHCP relay is required when the server is not on the same subnet as clients  
- DNS server IP can be internal or public (e.g., 8.8.8.8)


## Outcome 
![Image](https://github.com/user-attachments/assets/c347ff60-a1b8-44f0-b8b5-989823cdf7d2)
--
![Image](https://github.com/user-attachments/assets/4347e808-56c8-412a-ad3e-8bda02ae8133)
