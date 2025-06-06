# Static, Default & Floating Routing – Manual Route Control

## Objective  
Manually configure static, default, and floating routes on Cisco routers. Learn how to control path selection and redundancy without using dynamic protocols.

---

## Key Concepts

- **Static Route**: Manual path to a specific network  
- **Default Route (`0.0.0.0/0`)**: Catch-all path for unknown destinations  
- **Floating Static**: Backup route with higher administrative distance (AD)

---

## Basic Topology

```
PC0 --- R1 --- R2 --- PC1
```

- PC0 → 192.168.1.10/24  
- PC1 → 192.168.3.10/24  
- R1 ↔ R2: 192.168.2.0/30  
- R1 g0/0: 192.168.1.1  
- R1 g0/1: 192.168.2.1  
- R2 g0/0: 192.168.2.2  
- R2 g0/1: 192.168.3.1

---

## Static Routing Configuration

### On R1:
```bash
ip route 192.168.3.0 255.255.255.0 192.168.2.2
```

### On R2:
```bash
ip route 192.168.1.0 255.255.255.0 192.168.2.1
```

---

## Default Route

### On R1:
```bash
ip route 0.0.0.0 0.0.0.0 192.168.2.2
```
- R1 will forward all unknown traffic to R2.

---

## Floating Static Route

Used as a backup. Add higher AD value.

```bash
ip route 192.168.3.0 255.255.255.0 192.168.2.2 5
```
- This will only be used if a dynamic or primary static route fails.

---

## Verification

```bash
show ip route
ping 192.168.3.10
traceroute 192.168.3.10
```

Check route codes:  
- `S` = Static  
- `*` = candidate default  
- `AD` = administrative distance

---

## Tips

- Use `ping` and `traceroute` to confirm path  
- Keep floating routes inactive by using a **higher AD**  
- Combine with `show ip protocols` to compare static vs dynamic

