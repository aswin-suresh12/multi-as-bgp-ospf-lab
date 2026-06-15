# multi-as-bgp-ospf-lab
Multi-AS BGP and OSPF routing lab implemented in GNS3 using Cisco IOS routers.
## Overview

This project demonstrates a multi-AS routing design using Cisco IOS routers in GNS3.

### Autonomous Systems

- AS100 - R1
- AS200 - R2, R3, R4
- AS300 - R5

### Technologies

- OSPF Area 0
- eBGP
- iBGP Full Mesh
- Loopback Interfaces
- Next-Hop-Self
- Route Propagation
- Network Troubleshooting

## Topology
<img width="864" height="557" alt="BGP LAB PNG" src="https://github.com/user-attachments/assets/c9a3c20d-9961-40df-9dd9-c3a1d7a13afb" />

## Key Objectives

- Establish OSPF within AS200
- Configure eBGP between AS100 and AS200
- Configure eBGP between AS200 and AS300
- Implement iBGP full mesh inside AS200
- Verify end-to-end connectivity

## Troubleshooting

### Issue

Route 10.1.1.0/24 appeared in the BGP table but was not installed into the routing table in R4.

### Root Cause

The BGP next-hop attribute pointed to an unreachable address.

### Resolution

On R2 Configured:

```cisco
neighbor 3.3.3.3 next-hop-self
neighbor 4.4.4.4 next-hop-self
```

### OSPF DR Election Observation

R2 initially became the DR because OSPF was enabled on R2 before R3 joined the segment. Since OSPF DR elections are non-preemptive, R3 did not automatically take over the DR role despite having a higher Router ID (3.3.3.3). After clearing the OSPF process and triggering a fresh election, R3 was elected DR and R2 became BDR.

## Verification

Successful connectivity:

PC1 (10.1.1.10)
     ↓
R1 → R2 → R3 → R4 → R5
     ↓
PC2 (192.168.1.10)
```

PC1> ping 192.168.1.10
84 bytes from 192.168.1.10 icmp_seq=1 ttl=59 time=153.352 ms
84 bytes from 192.168.1.10 icmp_seq=2 ttl=59 time=165.428 ms
84 bytes from 192.168.1.10 icmp_seq=3 ttl=59 time=154.451 ms
84 bytes from 192.168.1.10 icmp_seq=4 ttl=59 time=153.163 ms
84 bytes from 192.168.1.10 icmp_seq=5 ttl=59 time=138.242 ms

PC1> trace 192.168.1.10
trace to 192.168.1.10, 8 hops max, press Ctrl+C to stop
 1   10.1.1.1   10.729 ms  16.056 ms  15.156 ms
 2   20.1.1.30   46.105 ms  46.129 ms  46.600 ms
 3   30.1.1.20   76.204 ms  76.851 ms  60.999 ms
 4   50.1.1.20   108.469 ms  106.822 ms  106.875 ms
 5   100.1.1.30   136.616 ms  138.946 ms  138.542 ms
 6   *192.168.1.10   140.216 ms (ICMP type:3, code:3, Destination port unreachable) (Higher UDP )

Ping and traceroute verification completed successfully.
