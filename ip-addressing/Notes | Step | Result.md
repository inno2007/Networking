# IP Addressing – Classes, Binary, Private/Public, Pinging

## Objective  
Understand how IP addresses work, including binary conversion, identifying ranges, and assigning static addresses in a LAN. Test connectivity using basic commands.

---

## Key Concepts

- IP Classes: A / B / C  
- IP Types: Private, Public, Unicast, Broadcast, Multicast  
- Binary ↔ Decimal conversion  
- Identify:
  - Network Address
  - First Host
  - Last Host
  - Broadcast Address

---

## Tasks

1. Convert IPs between binary and decimal


2. Classify the IP:
   - Class A: 1.0.0.0 to 126.255.255.255  
   - Class B: 128.0.0.0 to 191.255.255.255  
   - Class C: 192.0.0.0 to 223.255.255.255
  
 
3. Determine:
   - Network Address  
   - First/Last usable host  
   - Broadcast Address
  

4. Identify public vs private addresses  
   - Private IP Ranges:  
     - 10.0.0.0 – 10.255.255.255  
     - 172.16.0.0 – 172.31.255.255  
     - 192.168.0.0 – 192.168.255.255  

---

## Packet Tracer Practice

### Build a basic LAN:
- 2 PCs + 1 Switch

### Steps:
1. Assign static IPs:
   - PC0: 192.168.1.1 /24  
   - PC1: 192.168.1.2 /24

2. Set subnet mask:
   ```
   255.255.255.0
   ```

3. Test with:
   ```bash
   ping 192.168.1.2
   ```

- If successful, Layer 3 connectivity is working!

---

##  Tips

- The **last address** is always broadcast  
- The **first usable** is one after the network address  
- Ping helps verify physical + logical connectivity  
- Always confirm subnet matches between hosts

