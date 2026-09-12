# IP Addressing Plan

## Infrastructure links

| Link/device | Interface | Address | Mask / prefix | Gateway |
|---|---|---|---|---|
| ISP-R1 to EDGE-R1 | ISP `G0/0` | `203.0.113.1` | `/30` | — |
| ISP-R1 to EDGE-R1 | EDGE `G0/0` | `203.0.113.2` | `/30` | `203.0.113.1` |
| EDGE-R1 to CORE-SW1 | EDGE `G0/1` | `10.20.254.1` | `/30` | — |
| EDGE-R1 to CORE-SW1 | CORE `G0/1` | `10.20.254.2` | `/30` | `10.20.254.1` |
| Simulated Internet | ISP `G0/1` | `198.51.100.1` | `/24` | — |
| INTERNET-SRV | NIC | `198.51.100.10` | `/24` | `198.51.100.1` |

`203.0.113.0/24` and `198.51.100.0/24` are documentation networks, making them safe choices for a simulated lab.

## VLAN networks

| VLAN | Network | Default gateway | Allocation approach |
|---:|---|---|---|
| 10 | `10.20.10.0/24` | `10.20.10.1` | Static management addresses |
| 20 | `10.20.20.0/24` | `10.20.20.1` | DHCP from `.50` |
| 30 | `10.20.30.0/24` | `10.20.30.1` | DHCP from `.50` |
| 40 | `10.20.40.0/24` | `10.20.40.1` | DHCP from `.50` |
| 50 | `10.20.50.0/24` | `10.20.50.1` | DHCP from `.50`; reserve low addresses for administrators |
| 60 | `10.20.60.0/24` | `10.20.60.1` | DHCP from `.50` |
| 70 | `10.20.70.0/24` | `10.20.70.1` | Static servers |
| 80 | `10.20.80.0/24` | `10.20.80.1` | Static printers |
| 90 | `10.20.90.0/24` | `10.20.90.1` | Static cameras/IoT |
| 100 | `10.20.100.0/24` | `10.20.100.1` | DHCP from `.50` |
| 999 | No Layer 3 network | None | Native/parking VLAN only |

## Static device inventory

| Device | Address | VLAN | Notes |
|---|---|---:|---|
| CORE-SW1 | `10.20.10.1` | 10 | Also the management gateway |
| F1-SW1 | `10.20.10.11` | 10 | Floor 1 access switch |
| F2-SW1 | `10.20.10.12` | 10 | Floor 2 access switch |
| F3-SW1 | `10.20.10.13` | 10 | Floor 3 access switch |
| SERVER-SW1 | `10.20.10.14` | 10 | Server access switch |
| DNS-DHCP-SRV | `10.20.70.10` | 70 | Central DNS and DHCP |
| INTRANET-SRV | `10.20.70.20` | 70 | Internal HTTP service |
| NVR-SRV | `10.20.70.30` | 70 | Camera destination and optional logging |
| F1-PRN | `10.20.80.11` | 80 | Floor 1 printer |
| F2-PRN | `10.20.80.12` | 80 | Floor 2 printer |
| F3-PRN | `10.20.80.13` | 80 | Floor 3 printer |
| F1-AP | `10.20.100.11` | 100 | Guest access point in the simplified lab |
| F1-CAM | `10.20.90.11` | 90 | Sample camera |
| F2-CAM | `10.20.90.12` | 90 | Sample camera |
| F3-CAM | `10.20.90.13` | 90 | Sample camera |

## Addressing conventions

- `.1` is the gateway in each VLAN.
- `.2–.49` are reserved for network equipment and static infrastructure.
- `.50–.199` are available for DHCP clients.
- `.200–.239` are reserved for future static assignments.
- `.240–.254` remain unused for growth or temporary troubleshooting.
