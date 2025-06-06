# DHCPv4 & WLAN Concepts – Cisco Networking Theory

This document outlines how Dynamic Host Configuration Protocol (DHCPv4) and Wireless Local Area Networks (WLANs) work in Cisco-based networks, including configuration, verification, and wireless standards.

---

## Part 1: DHCPv4 – Dynamic Host Configuration Protocol

### What Is DHCPv4?

DHCP automatically assigns:
- IP Address  
- Subnet Mask  
- Default Gateway  
- DNS Server

This simplifies network setup and management, especially in large or dynamic environments.

---

### DHCPv4 Operation – DORA Process

1. **Discover**: Client sends a broadcast to find DHCP servers.  
2. **Offer**: Server replies with a proposed IP and lease info.  
3. **Request**: Client accepts the offer and requests assignment.  
4. **ACK**: Server confirms assignment and finalizes the lease.

---

### Basic DHCPv4 Server Configuration (Cisco IOS)

```bash
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.10.254

ip dhcp pool LAN-POOL-1
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.11.5
 domain-name example.com
```

---

### DHCP Verification & Maintenance

```bash
show running-config | section dhcp
show ip dhcp binding
show ip dhcp server statistics
```

To disable/re-enable DHCP:
```bash
no service dhcp
service dhcp
```

---

### DHCP Relay (Helper Address)

Used when DHCP server is not on the same subnet.

```bash
interface g0/0
 ip helper-address 192.168.11.6
```

This relays broadcasts (e.g., DHCPDISCOVER) to the specified DHCP server.

---

### DHCPv4 Client Configuration (Router Gets IP via DHCP)

```bash
interface g0/1
 ip address dhcp
```

Common for routers in SOHO environments or connecting to ISP.

---

## Part 2: WLAN – Wireless Networking Concepts

### Wireless Network Types

| Type  | Range           | Standard    | Use Case                      |
|-------|------------------|-------------|-------------------------------|
| WPAN  | 10–30 feet       | 802.15      | Bluetooth devices             |
| WLAN  | Up to 300 feet   | 802.11      | Wi-Fi (home/office)          |
| WMAN  | City-wide        | Various     | Municipal networks            |
| WWAN  | National/global  | LTE/5G      | Mobile internet (cellular)   |

---

### Wireless Frequencies

- **2.4 GHz** – Longer range, more interference  
- **5 GHz** – Shorter range, faster speeds

Used by:
- 2.4 GHz: 802.11b/g/n
- 5 GHz: 802.11a/n/ac/ax

---

### WLAN Components

| Component         | Role                                    |
|------------------|------------------------------------------|
| Wireless NIC      | Found in client devices                 |
| Access Point (AP) | Connects clients wirelessly             |
| Wireless Router   | Combines AP, switch, and routing        |

---

### AP Types

- **Autonomous AP**: Configured manually (CLI/GUI)  
- **Lightweight AP (LAP)**: Managed via Wireless LAN Controller (WLC)

---

### Wireless Topology Modes

- **Ad Hoc**: Peer-to-peer (no AP)  
- **Infrastructure**: Clients connect through AP  
- **Tethering**: Mobile hotspot (form of ad hoc)

---

### Wireless Frame & Association

Clients associate with an AP using:
- Matching **SSID**  
- Compatible **security mode**  
- Shared **passphrase**  
- Negotiated **802.11 standard**

---

### Active vs Passive Discovery

| Mode     | Description                            |
|----------|----------------------------------------|
| Passive  | AP sends **beacon** frames             |
| Active   | Client sends **probe requests**        |

---

### WLAN Security Options

| Method               | Description                            |
|----------------------|----------------------------------------|
| SSID Cloaking        | Hide network name                      |
| MAC Filtering         | Allow only specific device MACs        |
| WPA2/WPA3            | Encrypt traffic using shared key       |

---

### Remote Site Wireless Router Setup

1. Log in via browser (default IP/password)  
2. Change admin password  
3. Modify DHCP settings  
4. Configure SSID, channel, and security mode (WPA2)  
5. Reconnect using new wireless settings  

---

## Summary

| Feature     | DHCPv4                            | WLAN                                |
|-------------|-----------------------------------|--------------------------------------|
| Purpose     | Automate IP configuration         | Enable wireless access               |
| Key Tools   | Pools, Exclusions, Helper Address | SSID, APs, Channels, Security Modes  |
| Use Case    | Large networks, SOHO, ISP links   | Home/Enterprise wireless access      |

