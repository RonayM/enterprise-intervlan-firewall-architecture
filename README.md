Security Context

Enterprise-style internal network with controlled inter-VLAN communication, firewall enforcement, and simulated external connectivity.

This scenario builds upon a basic network by introducing segmentation, access control, and security zoning, reflecting real-world enterprise and financial environments.

Network Overview

Medium enterprise internal network

Multiple security zones implemented using VLANs

Dedicated firewall router controlling traffic between zones

ISP router simulating an external / untrusted network

Centralized DHCP service

Explicit security policy enforcement using ACLs

Network Topology
VLANs and Addressing
VLAN	Role	Network
VLAN 10	Users	192.168.20.0/24
VLAN 30	Server Zone	192.168.30.0/24
Key Devices

Firewall Router (Router-on-a-Stick)

ISP Router (External network simulation)

Layer 2 Switch

Dedicated Server

User Workstations (4 implemented in lab)

IP Addressing Scheme
Firewall Router

Gi0/0.10 (Users): 192.168.20.1

Gi0/0.30 (Server): 192.168.30.1

Gi0/1 (ISP link): 192.168.10.254

ISP Router

Gi0/0 (to Firewall): 192.168.10.1

Server

Static IP: 192.168.30.2

DHCP Configuration

DHCP service is provided by the Firewall Router

DHCP scope:

Network: 192.168.20.0/24

Default Gateway: 192.168.20.1

Client IP addresses are assigned dynamically

Infrastructure IP addresses are excluded from the DHCP pool

Firewall & Access Control
Security Policy

✅ Users are allowed to access the server only via HTTP (TCP 80) and HTTPS (TCP 443)

❌ All other traffic to the server is denied

❌ ICMP (ping) traffic to the server is blocked

✔ Traffic is filtered at the firewall boundary, not at the host level

Firewall Implementation

Extended ACL applied inbound on the Users VLAN interface

Default-deny model with explicit allow rules

ACL Logic (Conceptual)
Allow: TCP 80, 443 → Server
Deny: All other traffic → Server
Permit: All remaining traffic

Routing Design
Internal Routing

Inter-VLAN routing implemented using Router-on-a-Stick

IEEE 802.1Q trunk between switch and firewall router

External Routing

ISP Router configured with static routes pointing to internal networks

Firewall Router uses ISP Router as next-hop toward external networks

Observation

All client machines successfully obtain IP addresses via DHCP

Users can access the server only through approved services (HTTP/HTTPS)

ICMP and unauthorized protocols are blocked by the firewall

VLAN segmentation prevents lateral movement

External connectivity is logically separated from internal zones

Security Analysis
Identified Protections

Network segmentation using VLANs

Centralized firewall policy enforcement

Least-privilege access model

Trusted vs untrusted zone separation

No direct user access to server services beyond required ports

Remaining Limitations

No NAT implemented (internet access not required for this scenario)

No IDS/IPS or application-layer firewall

No identity-based access control (AAA / 802.1X)

Design Decision

Use VLAN segmentation instead of a flat network

Enforce access control at the firewall rather than at endpoints

Separate internal resources from simulated external connectivity

Intentionally block ICMP to reduce network reconnaissance

Keep the architecture simple but security-focused

Result

Network operates reliably and predictably

Security policies are enforced exactly as designed

Unauthorized access attempts are blocked

The lab accurately represents a controlled enterprise internal network

Lab Implementation

Implemented using Cisco Packet Tracer

Devices Used

1× Router (Firewall – Router-on-a-Stick)

1× Router (ISP Simulation)

1× Switch (Layer 2)

1× Server

4× Client PCs
