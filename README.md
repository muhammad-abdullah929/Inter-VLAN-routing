# Inter-VLAN Routing Project (Router-on-a-Stick)

Cisco Packet Tracer lab demonstrating inter-VLAN routing using a single router with sub-interfaces (router-on-a-stick).

## Topology

- **Router1** (Cisco 2911) — trunk link to Switch0, routes between VLANs
- **Switch0** (Cisco 2960-24TT) — Layer 2 switch, VLANs + 802.1Q trunk
- **PC0–PC5** — end hosts distributed across three VLANs

```
                [Router1]
                    |
                (trunk Fa0/24)
                    |
                [Switch0]
        /     /     |     \     \     \
     PC0   PC1     PC2    PC3   PC4   PC5
```

## VLAN / IP Plan

| VLAN | Name        | Network          | Gateway        | Ports        |
|------|-------------|------------------|----------------|--------------|
| 10   | Faculty/Data| 192.168.10.0/24  | 192.168.10.1   | Fa0/1–Fa0/2  |
| 20   | Sales       | 192.168.20.0/24  | 192.168.20.1   | Fa0/3–Fa0/4  |
| 30   | Management  | 192.168.30.0/24  | 192.168.30.1   | Fa0/5–Fa0/6  |

## Switch0 Configuration

```
vlan 10
vlan 20
vlan 30

interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10

interface FastEthernet0/3
 switchport mode access
 switchport access vlan 20

interface FastEthernet0/5
 switchport mode access
 switchport access vlan 30

interface FastEthernet0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
```

## Router1 Configuration

```
interface GigabitEthernet0/0
 no shutdown

interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
```

## PC IP Assignment

Each PC is statically configured within its VLAN's subnet, with the matching router sub-interface as default gateway (e.g., PC1 → `192.168.10.10/24`, gateway `192.168.10.1`).

## Verification

- Same-VLAN ping: switched locally, successful
- Cross-VLAN ping: routed via Router1, successful
- `show ip interface brief` on Router1: all sub-interfaces up/up
- `show vlan brief` / `show interface trunk` on Switch0: confirms VLANs and trunk state

## Files

- `Inter-VLAN_Routing_Report.docx` — full write-up with screenshots and explanations

## Notes

VLAN names/subnets above are placeholders — update to match your exact `.pkt` file if they differ.
