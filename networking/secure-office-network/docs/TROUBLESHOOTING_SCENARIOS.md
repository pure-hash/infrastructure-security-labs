# Controlled Troubleshooting Scenarios

Complete at least two scenarios after the working baseline is saved. Make a backup `.pkt` file before intentionally breaking anything.

## Scenario 1 — DHCP relay failure

### Introduce the fault

Remove the helper address from VLAN 20:

```text
CORE-SW1(config)# interface vlan 20
CORE-SW1(config-if)# no ip helper-address 10.20.70.10
```

### Expected symptom

The Sales PC fails to obtain an address, while clients in other VLANs continue working.

### Investigation

```text
show running-config interface vlan 20
show ip interface brief
```

Use Simulation mode to observe the DHCP Discover remain a broadcast in VLAN 20 instead of being relayed to the server.

### Repair

```text
interface vlan 20
 ip helper-address 10.20.70.10
```

Renew the client address and record the successful DORA exchange.

## Scenario 2 — Native VLAN mismatch

### Introduce the fault

Change the native VLAN on one side of the F1 trunk to VLAN 1.

### Expected symptom

CDP reports a native VLAN mismatch. Untagged/control traffic behavior is inconsistent, even if some tagged VLANs still work.

### Investigation

```text
show interfaces trunk
show cdp neighbors detail
```

Compare both ends of the link instead of changing random access ports.

### Repair

Restore VLAN 999 as the native VLAN on both ends.

## Scenario 3 — Incorrect DNS record

### Introduce the fault

Change `intranet.evanlab.local` from `10.20.70.20` to `10.20.70.30` on the DNS server.

### Expected symptom

Users can ping both servers by IP, but the intranet hostname opens the wrong service.

### Investigation

```text
nslookup intranet.evanlab.local
ping 10.20.70.20
```

This demonstrates that successful IP connectivity does not prove correct name resolution.

### Repair

Restore the A record to `10.20.70.20` and retest.

## Scenario 4 — Wrong endpoint gateway

### Introduce the fault

Give a static printer the correct IP and mask but set its gateway to `10.20.80.254`.

### Expected symptom

Same-VLAN communication works, but other VLANs cannot reliably communicate with the printer because its return traffic uses the wrong gateway.

### Investigation

Compare the printer configuration with the VLAN addressing table and test same-subnet versus remote-subnet traffic.

### Repair

Set the gateway to `10.20.80.1`.

## Scenario 5 — Overly broad guest access

### Introduce the fault

Temporarily remove the internal-network deny statement from `GUEST-IN`.

### Expected symptom

Guest clients can reach the intranet and other internal networks.

### Investigation

```text
show access-lists GUEST-IN
show running-config interface vlan 100
```

Review rule order and confirm that the ACL is applied inbound on the guest SVI.

### Repair

Restore the deny statement before the final permit and verify that the deny counter increases.

## Write-up standard

For each completed scenario, include:

- User-facing symptom
- Network scope of impact
- Initial hypothesis
- Commands and evidence
- Root cause
- Exact repair
- Successful retest
- One prevention or monitoring recommendation
