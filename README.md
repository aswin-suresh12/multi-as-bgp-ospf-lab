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



## Key Objectives

- Establish OSPF within AS200
- Configure eBGP between AS100 and AS200
- Configure eBGP between AS200 and AS300
- Implement iBGP full mesh inside AS200
- Verify end-to-end connectivity

## Troubleshooting

### Issue

Route 10.1.1.0/24 appeared in the BGP table but was not installed into the routing table.

### Root Cause

The BGP next-hop attribute pointed to an unreachable address.

### Resolution

Configured:

```cisco
neighbor 3.3.3.3 next-hop-self
neighbor 4.4.4.4 next-hop-self
```

## Verification

Successful connectivity:

PC1 (10.1.1.10)
     ↓
R1 → R2 → R3 → R4 → R5
     ↓
PC2 (192.168.1.10)
```

Ping and traceroute verification completed successfully.
