# Packet Tracer Build Guide

## 1. Create the physical topology

Place the following devices and rename them immediately:

| Quantity | Packet Tracer device | Names |
|---:|---|---|
| 2 | Cisco 2911 Router | `ISP-R1`, `EDGE-R1` |
| 1 | Cisco 3560-24PS Multilayer Switch | `CORE-SW1` |
| 4 | Cisco 2960 Switch | `F1-SW1`, `F2-SW1`, `F3-SW1`, `SERVER-SW1` |
| 4 | Server | `DNS-DHCP-SRV`, `INTRANET-SRV`, `NVR-SRV`, `INTERNET-SRV` |
| 5+ | PC | At least one Sales, Operations, Finance, IT, and Guest client |
| 1 | Access Point | `F1-AP` |
| Optional | Printer, camera, IP phone, laptop | Representative endpoints |

Arrange the routers and core at the top, place one access switch per floor beneath the core, and place the server switch beside them. Use colored rectangles or Packet Tracer clusters to label the WAN, server room, Floor 1, Floor 2, and Floor 3.

Cable the devices exactly as shown in `PHYSICAL_PORT_MAP.md`.

## 2. Configure the infrastructure

Open each Cisco device, select **CLI**, enter initial configuration mode if prompted, and paste its matching file from `configs/`.

Recommended order:

1. `ISP-R1.cfg`
2. `EDGE-R1.cfg`
3. `CORE-SW1.cfg`
4. `SERVER-SW1.cfg`
5. `F1-SW1.cfg`
6. `F2-SW1.cfg`
7. `F3-SW1.cfg`

Packet Tracer sometimes pauses during RSA key generation. Wait for the command to complete before continuing. The passwords in the configuration files are deliberately obvious lab placeholders—never reuse real passwords in a public repository.

If your selected model uses different interface names, run:

```text
show ip interface brief
```

Then adjust the interface names while keeping the logical connections identical.

## 3. Verify Layer 2 before configuring endpoints

On `CORE-SW1`:

```text
show vlan brief
show interfaces trunk
show spanning-tree root
show ip interface brief
```

On each access switch:

```text
show interfaces trunk
show vlan brief
ping 10.20.10.1
```

Expected results:

- Each switch uplink is trunking.
- VLAN 999 is the native VLAN on both sides of every trunk.
- Only the VLANs required by that floor are allowed.
- The management SVI is up/up.
- Each access switch can ping the core management address.

If a management SVI is down, confirm that VLAN 10 exists, is allowed on the trunk, and has at least one active forwarding port.

## 4. Configure servers and endpoints

Follow `SERVER_AND_ENDPOINT_SETUP.md` to configure:

- Static server addresses
- DHCP pools
- DNS A records
- Internal and simulated public HTTP pages
- Representative clients, printers, cameras, and guest wireless

Configure only one endpoint per major VLAN at first. Add more devices after validation if the topology needs to look fuller for screenshots.

## 5. Validate inter-VLAN routing

From a DHCP-enabled Sales PC:

```text
ipconfig /all
ping 10.20.20.1
ping 10.20.70.10
ping 10.20.70.20
```

The PC should receive a `10.20.20.50+` address and reach the server VLAN through `CORE-SW1`.

On the core:

```text
show ip route
```

All VLAN networks should appear as connected routes, and `0.0.0.0/0` should point to `10.20.254.1`.

## 6. Validate DNS and HTTP

From an internal PC:

```text
nslookup intranet.evanlab.local
ping intranet.evanlab.local
```

Open **Desktop → Web Browser** and browse to:

```text
http://intranet.evanlab.local
```

The internal page should load from `10.20.70.20`.

## 7. Validate Internet simulation and PAT

From an internal PC:

```text
ping 198.51.100.10
```

Browse to:

```text
http://internet.test
```

On `EDGE-R1`:

```text
show ip nat translations
show ip nat statistics
```

You should see inside-local `10.20.x.x` addresses translated to the outside interface address `203.0.113.2`.

## 8. Validate security policy

Run the following tests:

- Guest client → `10.20.70.20`: blocked
- Guest client → `10.20.70.10` using DNS: allowed
- Guest client → `198.51.100.10`: allowed
- Camera → `10.20.70.30`: allowed
- Camera → an employee PC: blocked
- IT PC → `10.20.10.11` using SSH: allowed
- Sales PC → `10.20.10.11` using SSH: blocked

Review counters on the core:

```text
show access-lists
```

The ACL match counts are strong evidence that your policy is being enforced.

## 9. Use Simulation mode for evidence

Switch from **Realtime** to **Simulation** and filter for:

- ARP
- DHCP
- DNS
- ICMP
- TCP
- HTTP

Good evidence sequences include:

1. DHCP Discover → Offer → Request → ACK across a relay
2. DNS query and response followed by TCP/HTTP traffic
3. Guest packet being dropped by the Layer 3 ACL
4. Internal client crossing the edge and creating a NAT translation

Use **Capture/Forward** to advance one event at a time and open the PDU details for screenshots.

## 10. Save and document

Save the completed file as:

```text
packet-tracer/your-lab-example.pkt
```

Then complete the validation table in the main README, add screenshots, and record at least two troubleshooting scenarios in your own words.
