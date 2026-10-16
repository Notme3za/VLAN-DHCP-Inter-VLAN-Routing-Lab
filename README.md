# VLAN, DHCP & Inter-VLAN Routing Lab

A Cisco Packet Tracer lab that splits a small network into two VLANs, hands out addresses with DHCP from the router, and routes between the VLANs over a single trunk link (router-on-a-stick).

> This is a simulated lab built in Cisco Packet Tracer, not real hardware.

## Topology

![Network topology: one router, one switch, four PCs](images/01-topology.png)

| Device | Model | Role |
|---|---|---|
| Router0 | Cisco 2911 | Default gateway for both VLANs and DHCP server |
| Switch0 | Cisco 2960-24TT | Connects the PCs and the router |
| PC0 to PC3 | PC-PT | Clients |

## Addressing

| VLAN | Name | Subnet | Default gateway |
|---|---|---|---|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 |

## Design

- **Two VLANs** (10 and 20) put HR and IT in separate broadcast domains.
- **One 802.1Q trunk** carries both VLANs between the switch and the router.
- **Router-on-a-stick:** the router has one logical sub-interface per VLAN, each acting as that VLAN's default gateway, so traffic between the VLANs is routed over the single physical link.
- **DHCP on the router:** one pool per VLAN gives each client an address and the matching default gateway.

## Verification

### DHCP leases

`show ip dhcp binding` on the router shows a lease from the correct pool for each VLAN. The configuration was then saved with `copy running-config startup-config`.

| VLAN | Host | Leased address |
|---|---|---|
| 10 (HR) | PC0 | 192.168.10.2 |
| 20 (IT) | PC2 | 192.168.20.2 |

![show ip dhcp binding output on the router](images/02-dhcp-bindings.png)

Both bindings are of type *Automatic*, meaning the router assigned them dynamically from its pools. Each PC received an address from its own VLAN's subnet.

<!--
Uncomment this section after running the tests in Packet Tracer and pasting REAL output.

### Connectivity tests

| Test | Command (from the PC's Command Prompt) | Result |
|---|---|---|
| Same VLAN | `ping <address of the other HR PC>` from PC0 | <paste result> |
| Across VLANs | `ping 192.168.20.2` from PC0 | <paste result> |
| Path taken | `tracert 192.168.20.2` from PC0 | <paste result> |

Note: the first ping in Packet Tracer often times out while ARP resolves. That is normal.
-->

## Files

- [`vlan-dhcp-routing-lab.pkt`](vlan-dhcp-routing-lab.pkt): the Packet Tracer lab. Open it with Cisco Packet Tracer.
- [`images/`](images/): the screenshots used above.

## Key concepts

- A VLAN is a separate broadcast domain. Devices in different VLANs can't talk directly, so a router (layer 3) is needed between them.
- A trunk carries several VLANs over one link by tagging each frame (802.1Q).
- Router-on-a-stick splits one physical router interface into sub-interfaces, one per VLAN.
- Each DHCP pool matches one subnet and hands out that subnet's default gateway.

## Limitations and next steps

- Everything here is simulated in Packet Tracer.
- No access control lists are configured, so by default HR and IT can reach each other. Restricting that traffic with ACLs is the natural next step.
- Add a third VLAN and document the extra steps.
