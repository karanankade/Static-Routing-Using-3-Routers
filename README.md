# Static-Routing-Using-3-Routers
Lab Report: Static Routing Using 3 Routers

1. Objective

To configure static routing between three routers using a subnetted network 10.0.0.0/8 divided into /11 subnets, and to verify end‑to‑end connectivity between all LANs.

2. Topology Description

The network consists of:

3 Routers: Router0, Router1, Router2

3 Switches: Connecting the LAN devices to the routers

LAN 1: 3 PCs

LAN 2: 2 Laptops + 1 Server

LAN 3: 3 PCs

Topology Layout: Routers are connected in a linear topology (R0 — R1 — R2).

3. Subnetting Plan (10.0.0.0/8 → /11)

Subnet Mask Used: 255.224.0.0 (/11)

Subnet Network

Assignment

10.0.0.0/11

LAN 1

10.32.0.0/11

R0–R1 Link

10.64.0.0/11

LAN 2

10.96.0.0/11

R1–R2 Link

10.128.0.0/11

LAN 3

4. IP Addressing Table

Device

Interface / Hostname

IP Address

Subnet Mask

Default Gateway

Router0

Fa0/0 (LAN 1)

10.0.0.1

255.224.0.0

N/A



Fa0/1 (R0-R1)

10.32.0.1

255.224.0.0

N/A

Router1

Fa0/0 (R0-R1)

10.32.0.2

255.224.0.0

N/A



Fa0/1 (LAN 2)

10.64.0.1

255.224.0.0

N/A



Fa1/0 (R1-R2)

10.96.0.1

255.224.0.0

N/A

Router2

Fa0/0 (LAN 3)

10.128.0.1

255.224.0.0

N/A



Fa0/1 (R1-R2)

10.96.0.2

255.224.0.0

N/A

LAN 1

PC0

10.0.0.2

255.224.0.0

10.0.0.1



PC1

10.0.0.3

255.224.0.0

10.0.0.1



PC2

10.0.0.4

255.224.0.0

10.0.0.1

LAN 2

Laptop0

10.64.0.2

255.224.0.0

10.64.0.1



Server

10.64.0.3

255.224.0.0

10.64.0.1



Laptop1

10.64.0.4

255.224.0.0

10.64.0.1

LAN 3

PC3

10.128.0.3

255.224.0.0

10.128.0.1



PC4

10.128.0.2

255.224.0.0

10.128.0.1



PC5

10.128.0.4

255.224.0.0

10.128.0.1

5. Router Configuration Commands

Router0

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


Router1

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


Router2

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


6. Verification

6.1 Routing Table & Interface Verification

The outputs below confirm that the interfaces are up and that the static routes have been successfully added to the routing tables of each router.

Router0:


Router1:


Router2:


6.2 End-to-End Connectivity (Ping Tests)

To verify the configuration, ping tests were conducted across the network subnets.

Ping from LAN 1 (PC0): Successfully reaching devices in LAN 3.


Ping from LAN 2 (Laptop1): Successfully reaching devices in LAN 1 and LAN 3.


Ping from LAN 3 (PC5): Successfully reaching devices in LAN 1 and LAN 2.


Result: All pings were successful, indicating that the static routing is functioning properly.

7. Conclusion

Static routing was successfully implemented between three routers using /11 subnetting of the 10.0.0.0/8 network. Full, end-to-end connectivity between all LANs across the topology was successfully achieved.
