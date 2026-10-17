# Troubleshooting Report — DHCP & DNS Small Office Network

## 1. Objective

Document the configuration, verification, and troubleshooting process for the DHCP and DNS Small Office Network project built in Cisco Packet Tracer.

## 2. Network Summary

- Router: Cisco 2911 (`R1`) — planned
- Switch: Cisco 2960 (`SW1`) — planned
- Server: Server-PT (`Server1`) — planned
- Client devices: PC1 and PC2
- Network: `192.168.10.0/24`
- Default gateway: `192.168.10.1`
- DHCP server: `192.168.10.10`
- DNS server: `192.168.10.10`
- Planned DNS hostname: `intranet.office.test`

## 3. Verification Commands and Methods

### Client Verification

Use the PC command prompt to inspect the IP configuration:

```text
ipconfig
```

Use ping to test connectivity:

```text
ping 192.168.10.1
ping 192.168.10.10
```

Test DNS resolution after configuring the DNS service:

```text
ping intranet.office.test
```

### Router Verification

```text
show ip interface brief
show running-config
```

These commands help verify interface status, IP addressing, and the router configuration.

### DHCP and DNS Verification

Use the Server1 configuration window in Packet Tracer to inspect the DHCP and DNS service settings.

Verify that:
- DHCP is enabled and the address pool is correct.
- The default gateway and DNS server options are correct.
- DNS is enabled and the hostname record points to the intended IP address.
- Client PCs receive the expected network settings.

## 4. Connectivity Tests

| Test | Destination | Expected result | Actual result |
|---|---|---|---|
| DHCP assignment | DHCP server / client IP settings | Valid address from the configured pool | Pending |
| Gateway connectivity | `192.168.10.1` | Successful ping | Pending |
| Server connectivity | `192.168.10.10` | Successful ping | Pending |
| DNS resolution | `intranet.office.test` | Resolves to `192.168.10.10` | Pending |
| Client connectivity | PC1 to PC2 | Successful ping | Pending |

Update the actual results after performing each test in Packet Tracer.

## 5. Troubleshooting Checklist

If DHCP or DNS does not work, check:

1. Whether all required physical links are connected and operational.
2. Whether the router interface has the correct IP address and is enabled.
3. Whether Server1 has the correct static IP address, subnet mask, and default gateway.
4. Whether the DHCP service is enabled and the pool has available addresses.
5. Whether the DHCP pool provides the correct default gateway and DNS server.
6. Whether PC1 and PC2 are configured to obtain their IP addresses automatically.
7. Whether the DNS service is enabled and the hostname record is correct.
8. Whether the clients can reach the server and gateway using their IP addresses.
9. Whether the hostname resolves correctly after the DNS settings are corrected.

## 6. Troubleshooting Log

Record actual issues encountered during testing.

### Issue 1

- **Problem:** To be documented if encountered.
- **Cause:** To be identified from the observed evidence.
- **Solution:** To be documented after applying a fix.
- **Verification:** Record the test result after the fix.

## 7. Conclusion

Complete this section after implementing and testing the network. Summarize whether DHCP address assignment, gateway connectivity, server connectivity, and DNS name resolution worked as expected.

## Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- DHCP and DNS server services
