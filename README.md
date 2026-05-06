# Project 1: Multi-Site Enterprise Network

## Objective
Design, configure, and secure a corporate network spanning two 
geographical locations — a Headquarters (HQ) and a Remote Branch Office.

## Tools Used
- Cisco Packet Tracer
- Router model: Cisco 2911
- Switch model: Cisco 2960

## Network Design

### IP Addressing Scheme
| Device               | VLAN | IP Address      | Gateway       |
|----------------------|------|-----------------|---------------|
| HQ-Management-PC     | 10   | 192.168.10.2    | 192.168.10.1  |
| HQ-HR-PC             | 20   | 192.168.20.2    | 192.168.20.1  |
| HQ-Guest-PC          | 30   | 192.168.30.2    | 192.168.30.1  |
| Branch-Management-PC | 10   | 192.168.110.2   | 192.168.110.1 |
| Branch-HR-PC         | 20   | 192.168.120.2   | 192.168.120.1 |
| HQ-Router (WAN)      | -    | 10.0.0.1        | -             |
| Branch-Router (WAN)  | -    | 10.0.0.2        | -             |

## What I Built

### VLANs
Configured three VLANs on the HQ switch to segment traffic logically:
- VLAN 10 — Management
- VLAN 20 — HR
- VLAN 30 — Guest

### Inter-VLAN Routing
Implemented Router-on-a-Stick on the HQ-Router using sub-interfaces
(GigabitEthernet0/0.10, .20, .30) allowing controlled communication
between VLANs through a single physical interface.

### Dynamic Routing (OSPF)
Deployed OSPF (Process 1, Area 0) on both routers to automatically
establish routing tables between HQ and Branch over the WAN link
(10.0.0.0/30). Branch PCs can successfully ping HQ PCs confirming
full cross-site connectivity.

### Access Control Lists (ACLs)
Configured an extended ACL (BLOCK-GUEST) applied inbound on the
GigabitEthernet0/0.30 sub-interface to restrict Guest network traffic
from reaching the Management network, while allowing all other traffic.

## Verification
- HQ-Management-PC successfully pings HQ-HR-PC ✅
- Branch-Management-PC successfully pings HQ-Management-PC ✅
- HQ-Guest-PC cannot reach HQ-Management network (ACL blocking) ✅

## Packet Flow — Branch to HQ
A packet originating from Branch-Management-PC (192.168.110.2) travels
as follows:
1. Branch-Management-PC sends packet to its default gateway (192.168.110.1)
2. Packet arrives at Branch-Switch via FastEthernet0/1 (VLAN 10)
3. Branch-Switch forwards packet via trunk port to Branch-Router
4. Branch-Router forwards packet via WAN link (GigabitEthernet0/1) to HQ-Router
5. HQ-Router receives packet on GigabitEthernet0/1 and routes it to the
   destination subnet via OSPF learned routes
6. Packet is forwarded to HQ-Switch via trunk port
7. HQ-Switch delivers packet to HQ-Management-PC via FastEthernet0/1

## Files
- `Multi-Site-Enterprise-Network.pkt` — Packet Tracer topology file
- `topology-diagram.png` — Network topology diagram

## Key Learnings
- How VLANs segment traffic at Layer 2 to improve security and performance
- How Router-on-a-Stick enables inter-VLAN routing on a single interface
- How OSPF dynamically propagates routing tables between sites
- How extended ACLs enforce granular security policies at the network layer
