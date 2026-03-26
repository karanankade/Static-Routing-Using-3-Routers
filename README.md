# Static-Routing-Using-3-Routers
Static Routing Using 3 Routers

Overview

This repository contains the configuration details and documentation for a network topology that implements static routing across three routers. The project demonstrates subnetting a 10.0.0.0/8 network into /11 subnets and establishing successful end-to-end connectivity between multiple distinct Local Area Networks (LANs).

Topology

The network is structured in a linear topology (R0 — R1 — R2) and consists of:

3 Routers: Router0, Router1, Router2

3 Switches: Acting as the distribution point for each LAN

End Devices:

LAN 1: 3 PCs

LAN 2: 2 Laptops, 1 Server

LAN 3: 3 PCs

Subnetting Plan

The base network 10.0.0.0/8 is subnetted using a 255.224.0.0 (/11) subnet mask. The network assignments are as follows:

10.0.0.0/11 → LAN 1

10.32.0.0/11 → R0–R1 Link

10.64.0.0/11 → LAN 2

10.96.0.0/11 → R1–R2 Link

10.128.0.0/11 → LAN 3

Configuration Highlights

The routers are configured using standard Cisco IOS commands to set up interfaces and define static paths to non-directly connected networks.

Example static routing command structure used in this project:

ip route <destination_network> <subnet_mask> <next_hop_ip>


(For full interface IP assignments and specific routing commands for each router, refer to the detailed Lab Report.)

Verification & Testing

To verify the functionality of this setup in your own environment:

Routing Tables: Run show ip route on each router to ensure the static routes are present.

Interface Status: Run show ip interface brief to confirm all relevant interfaces are "up" and assigned the correct IP addresses.

End-to-End Connectivity: Open the command prompt on the end devices and execute ping tests between different LANs (e.g., Ping from LAN 1 to LAN 3).

Results

When configured correctly according to this documentation, full connectivity is achieved, and all ICMP ping requests between LAN 1, LAN 2, and LAN 3 are completely successful.
