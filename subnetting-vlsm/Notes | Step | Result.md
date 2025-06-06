# Subnetting & VLSM – Efficient IP Addressing

## Objective  
Learn how to break a network into smaller subnets using traditional subnetting and Variable Length Subnet Masking (VLSM). Efficiently assign IPs based on host requirements.

---

## Key Concepts

- Subnetting = Dividing a network into equal-size subnets  
- VLSM = Dividing into unequal sizes based on number of hosts  
- CIDR notation (/24, /27, etc.)  
- Calculate:
  - Subnet Mask  
  - Number of subnets  
  - Usable hosts per subnet  
  - Network and Broadcast addresses

---

## Example

Given:  
- Network: `192.168.1.0/24`  
- Host Requirements:
  - A: 50 hosts  
  - B: 20 hosts  
  - C: 10 hosts  

Apply VLSM:

| Subnet | Hosts | CIDR  | Range               |
|--------|-------|-------|---------------------|
| A      | 50    | /26   | 192.168.1.0 – .63   |
| B      | 20    | /27   | 192.168.1.64 – .95  |
| C      | 10    | /28   | 192.168.1.96 – .111 |

---

## Cisco Packet Tracer 

### Build a network:
- 1 Router  
- 3 Switches  
- 6 PCs (2 per subnet)

### Steps:
1. Assign IPs based on VLSM chart  
2. Configure router interfaces/sub-interfaces  
3. Set default gateways for each PC  
4. Use:
   ```bash
   ping <other PC IP>
   ```

- All PCs should reach each other if subnets and gateways are correctly assigned.

---

## Key Formulas

- Hosts per subnet = 2ⁿ – 2  
- Number of subnets = 2ⁿ (where n = borrowed bits)  
- Subnet increment = 256 – subnet mask value

---

## Tips

- Use the largest host requirement first  
- No overlap between subnet ranges  
- Assign lowest usable IP to gateway (router)  
- Document subnet chart before configuration
