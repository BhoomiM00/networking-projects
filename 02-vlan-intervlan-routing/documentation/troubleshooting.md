# Troubleshooting Report — VLAN & Inter-VLAN Routing

## 1. Objective

Document the verification and troubleshooting process for the VLAN Configuration and Inter-VLAN Routing project built in Cisco Packet Tracer.

## 2. Network Summary

- Router: Cisco 2911 (`R1`)
- Switch: Cisco 2960 (`SW1`)
- VLAN 10: Administration — `192.168.10.0/24`
- VLAN 20: IT — `192.168.20.0/24`
- Routing method: Router-on-a-stick using 802.1Q encapsulation

## 3. Verification Commands

### Switch Verification

```text
show vlan brief
show interfaces trunk
```

These commands were used to verify VLAN creation, access-port assignments, and trunk configuration.

### Router Verification

```text
show ip interface brief
```

This command was used to verify the router interface and subinterface IP addresses and operational status.

## 4. Connectivity Tests

| Test | Destination | Expected Result |
|---|---|---|
| Administration gateway | `192.168.10.1` | Successful |
| Administration to IT | `192.168.20.10` | Successful |
| Same-VLAN connectivity | `192.168.10.11` | Successful |

All listed connectivity tests succeeded during project verification.

## 5. Troubleshooting Checklist

If connectivity fails, check:

1. Whether the physical links are up.
2. Whether the PCs have the correct IP addresses, subnet masks, and gateways.
3. Whether VLANs exist and access ports belong to the correct VLANs.
4. Whether the switch-to-router link is configured as a trunk.
5. Whether the router subinterfaces have the correct 802.1Q VLAN IDs and gateway IP addresses.
6. Whether the router's physical interface is enabled.

## 6. Conclusion

The network was configured and tested successfully in Cisco Packet Tracer. Communication within the same VLAN and between VLANs was verified using ping.

## Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
