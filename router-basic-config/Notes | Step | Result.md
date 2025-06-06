# Basic Router Configuration – Hostname, IP, Banner, Passwords

## Objective  
Learn to configure a Cisco router’s identity, access control, and interface IP settings. Secure access using passwords and verify connectivity using basic commands.

---

## Key Concepts

- Hostname & Banner: Identifies the device  
- Line Passwords: Secures access (console, VTY)  
- Enable Secret: Protects privileged mode  
- Interface IP Assignment  
- Saving Configuration

---

## Commands Used

### Set Hostname and Banner
```bash
conf t
hostname R1
banner motd #Unauthorized access is prohibited#
```

---

### Set Passwords
```bash
line console 0
password cisco
login
exit

line vty 0 4
password telnet123
login
exit

enable secret class
```

---

### Configure Interface IP
```bash
interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

Repeat for other interfaces:
```bash
interface g0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
```

---

### Save Configuration
```bash
copy running-config startup-config
```
- This ensures settings remain after reboot.

---

## Verify Settings
```bash
show ip interface brief
show running-config
```

---

## Packet Tracer Task

Build a topology:
- 1 Router  
- 2 Switches  
- 2 PCs (per side)

Assign:
- PC0 → 192.168.1.10  
- PC1 → 192.168.2.10  
- Router interfaces:
  - g0/0 → 192.168.1.1  
  - g0/1 → 192.168.2.1  

Set default gateway on PCs:
```
192.168.1.1 or 192.168.2.1
```

Test:
```bash
ping 192.168.2.10
```
- If its successful, router config is correct.

---

## Tips

- Always use `no shutdown` on interfaces  
- Use `enable secret` (not just `enable password`)  
- Save often using `copy run start`  
- Use descriptive hostnames for routers and switches
