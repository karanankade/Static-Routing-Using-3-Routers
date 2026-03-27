# Static Routing Using 3 Routers

# Lab Report: Static Routing & DHCP Configuration Using 3 Routers

## 1. Objective
To configure static routing between three Cisco routers using a subnetted Class A network (10.0.0.0/8 divided into /11 subnets), implement DHCP to automatically assign IP addresses to LAN endpoints, and verify end‑to‑end connectivity across the network.

## 2. Topology Description
The network physical layout follows a linear topology (Router0 — Router1 — Router2) consisting of:
- **3 Routers**: Router0, Router1, Router2
- **3 Switches**: Connecting individual LAN endpoints to their respective routers
- **LAN 1**: 3 PCs
- **LAN 2**: 2 Laptops + 1 Server
- **LAN 3**: 3 PCs

## 3. Subnetting Plan
The base Class A network `10.0.0.0/8` is subnetted into 8 equal-sized subnets using a `/11` prefix length.
- **Subnet Mask**: `255.224.0.0`
- **Subnet Allocations**:
  1. `10.0.0.0/11`   → LAN 1
  2. `10.32.0.0/11`  → Router0 – Router1 Link
  3. `10.64.0.0/11`  → LAN 2
  4. `10.96.0.0/11`  → Router1 – Router2 Link
  5. `10.128.0.0/11` → LAN 3

## 4. IP Addressing Table

| Device | Interface / Component | IP Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **Router0** | Fa0/0 (LAN 1) | 10.0.0.1 | 255.224.0.0 | N/A |
| | Fa0/1 (to R1) | 10.32.0.1 | 255.224.0.0 | N/A |
| **Router1** | Fa0/0 (to R0) | 10.32.0.2 | 255.224.0.0 | N/A |
| | Fa1/0 (to R2) | 10.96.0.1 | 255.224.0.0 | N/A |
| | Fa0/1 (LAN 2) | 10.64.0.1 | 255.224.0.0 | N/A |
| **Router2** | Fa0/1 (to R1) | 10.96.0.2 | 255.224.0.0 | N/A |
| | Fa0/0 (LAN 3) | 10.128.0.1 | 255.224.0.0 | N/A |
| **LAN 1** | PCs 0, 1, 2 | DHCP Assigned (e.g., 10.0.0.x) | 255.224.0.0 | 10.0.0.1 |
| **LAN 2** | Laptops 0, 1, Server | DHCP Assigned (e.g., 10.64.0.x) | 255.224.0.0 | 10.64.0.1 |
| **LAN 3** | PCs 3, 4, 5 | DHCP Assigned (e.g., 10.128.0.x) | 255.224.0.0 | 10.128.0.1 |

*(Note: We will exclude the first 10 IPs in each subnet's DHCP pool for stable network device reservations like servers and gateways.)*

## 5. Router Configurations

### Router0 (R0)
```ios
enable
configure terminal

! Interface Configuration
interface fa0/0
 ip address 10.0.0.1 255.224.0.0
 no shutdown
 exit

interface fa0/1
 ip address 10.32.0.1 255.224.0.0
 no shutdown
 exit

! Static Routing
ip route 10.64.0.0 255.224.0.0 10.32.0.2
ip route 10.96.0.0 255.224.0.0 10.32.0.2
ip route 10.128.0.0 255.224.0.0 10.32.0.2

! DHCP Configuration for LAN 1
ip dhcp excluded-address 10.0.0.1 10.0.0.10
ip dhcp pool LAN1_POOL
 network 10.0.0.0 255.224.0.0
 default-router 10.0.0.1
 dns-server 8.8.8.8
 exit
```

### Router1 (R1)
```ios
enable
configure terminal

! Interface Configuration
interface fa0/0
 ip address 10.32.0.2 255.224.0.0
 no shutdown
 exit

interface fa1/0
 ip address 10.96.0.1 255.224.0.0
 no shutdown
 exit

interface fa0/1
 ip address 10.64.0.1 255.224.0.0
 no shutdown
 exit

! Static Routing
ip route 10.0.0.0 255.224.0.0 10.32.0.1
ip route 10.128.0.0 255.224.0.0 10.96.0.2

! DHCP Configuration for LAN 2
ip dhcp excluded-address 10.64.0.1 10.64.0.10
ip dhcp pool LAN2_POOL
 network 10.64.0.0 255.224.0.0
 default-router 10.64.0.1
 dns-server 8.8.8.8
 exit
```

### Router2 (R2)
```ios
enable
configure terminal

! Interface Configuration
interface fa0/1
 ip address 10.96.0.2 255.224.0.0
 no shutdown
 exit

interface fa0/0
 ip address 10.128.0.1 255.224.0.0
 no shutdown
 exit

! Static Routing
ip route 10.0.0.0 255.224.0.0 10.96.0.1
ip route 10.32.0.0 255.224.0.0 10.96.0.1
ip route 10.64.0.0 255.224.0.0 10.96.0.1

! DHCP Configuration for LAN 3
ip dhcp excluded-address 10.128.0.1 10.128.0.10
ip dhcp pool LAN3_POOL
 network 10.128.0.0 255.224.0.0
 default-router 10.128.0.1
 dns-server 8.8.8.8
 exit
```

## 6. End-Device Device DHCP Assignment Procedure
1. Open the **IP Configuration** or **Network Settings** of each PC/Laptop.
2. Toggle the radio button from **Static** to **DHCP**. 
3. *Note*: The server device in LAN 2 can either be put on DHCP entirely, or kept as static using one of the statically excluded IP addresses (`10.64.0.3` is highly recommended since it falls into the `10.64.0.1 - 10.64.0.10` exclusion range).  

## 7. Verification Steps
1. **DHCP Validation**:
   - Run `show ip dhcp binding` on routers to review active IP leases securely distributed to network clients.
   - Run `ipconfig` (PC) to verify valid endpoint IPs under the `255.224.0.0` subnet structure. 
2. **Routing Validation**:
   - Run `show ip route` explicitly on each router to check routing table accuracy with `S` (Static) labeled remote subnets properly populated.
3. **End-to-End Testing**:
   - Ping from LAN 1 to LAN 2 (`ping 10.64.0.x`) 
   - Ping from LAN 1 to LAN 3 (`ping 10.128.0.x`)
   - Ping from LAN 2 to LAN 3 (`ping 10.128.0.x`)
   - All diagnostic attempts should resolve optimally.

## 8. Conclusion
The network was successfully implemented using static routing architecture coupled with a structured Class A `/11` subnet scheme. Automatic IP assignments were actively introduced to the environment utilizing localized DHCP pools directly on Router ports—increasing device scalability, diminishing manual administrative burdens, and delivering complete, verified end-to-end multi-LAN visibility.
When configured correctly according to this documentation, full connectivity is achieved, and all ICMP ping requests between LAN 1, LAN 2, and LAN 3 are completely successful.
