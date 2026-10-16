Project 3: VLANs, DHCP & Inter-VLAN Routing

HR — VLAN 10
Network: 192.168.10.0/24
Gateway: 192.168.10.1

IT — VLAN 20
Network: 192.168.20.0/24
Gateway: 192.168.20.1

Verification

DHCP leases:
- HR (VLAN 10), PC0: 192.168.10.2
- IT (VLAN 20), PC2: 192.168.20.2

The router assigned each PC an address from its VLAN’s subnet and provided the matching default gateway