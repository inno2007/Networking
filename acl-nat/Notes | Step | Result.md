# Access Control Lists (ACL) and Network Address Translation (NAT)

## Objective  
Configure standard and extended ACLs to control traffic flow based on IP, protocol, and port. Set up NAT to translate private IP addresses into public ones for internet access.

---

## Key Concepts

### ACL
- **Standard ACL**: Filters only by source IP
- **Extended ACL**: Filters by source/destination IP, protocol, port
- **Numbered ACLs**: Standard (1–99), Extended (100–199)
- Applied to interfaces in inbound or outbound direction

### NAT
- **Static NAT**: One-to-one translation
- **Dynamic NAT**: Pool of IPs
- **PAT (NAT overload)**: Many-to-one translation using ports

---

## ACL Configuration Examples

### Standard ACL (Block PC0 from network)
```
access-list 1 deny 192.168.1.10
access-list 1 permit any

interface g0/1
ip access-group 1 out
```

### Extended ACL (Allow HTTP, deny others)
```
access-list 100 permit tcp any any eq 80
access-list 100 deny ip any any

interface g0/0
ip access-group 100 in
```

Use:
```
show access-lists
```

---

## NAT Configuration Examples

### Static NAT
```
ip nat inside source static 192.168.1.10 200.0.0.10
```

### Dynamic NAT (with pool)
```
ip nat pool NATPOOL 200.0.0.100 200.0.0.110 netmask 255.255.255.0
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 pool NATPOOL
```

### PAT (NAT Overload)
```
ip nat inside source list 1 interface g0/0 overload
```

---

## Interface Roles

Inside interface:
```
interface g0/1
ip nat inside
```

Outside interface:
```
interface g0/0
ip nat outside
```

---

## Verification

```
show access-lists
show ip nat translations
show ip nat statistics
ping and test browser access (in Packet Tracer)
```

---

## Notes

- ACLs process top to bottom, stop at first match  
- Standard ACLs should be applied close to destination  
- Extended ACLs should be applied close to source  
- NAT must have correct inside/outside roles  
- PAT is most common for home/small office internet access
