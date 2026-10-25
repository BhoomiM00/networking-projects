# Enterprise Company Network — Troubleshooting Report

## 1. Overview

This document describes the troubleshooting procedures used in the Enterprise Company Network project built using Cisco Packet Tracer.

The network includes VLAN segmentation, inter-VLAN routing, DHCP, OSPF, ACLs, SSH, and switch port security.

The objective is to identify network problems, investigate their causes, apply corrections, and verify that the network operates as intended.

## 2. Troubleshooting Methodology

The following methodology is used:

1. Identify the problem.
2. Check physical connections and interface status.
3. Verify IP addressing and VLAN assignments.
4. Check trunking and routing configurations.
5. Examine ACLs and security restrictions.
6. Apply the appropriate correction.
7. Retest the affected connection.
8. Document the result.

## 3. Common Network Problems and Solutions

### Issue 1: PC Not Receiving DHCP Address

**Symptoms:**
- PC displays an incorrect or missing IP address.
- PC cannot communicate with its default gateway.

**Possible causes:**
- Incorrect access VLAN.
- DHCP pool not configured correctly.
- Router interface is down.
- Port security violation.
- ACL blocking DHCP discovery traffic.

**Troubleshooting commands:**

```cisco
show ip dhcp binding
show ip dhcp pool
show vlan brief
show ip interface brief
show access-lists
```

**Corrective actions:**
- Verify the PC is connected to the correct VLAN.
- Check DHCP pool configuration and excluded addresses.
- Ensure router and switch interfaces are operational.
- Check port security status.
- Permit DHCP client discovery traffic through the relevant inbound ACLs.

**Verification:**
- Renew the PC's DHCP address.
- Confirm the assigned IP address, subnet mask, and default gateway.
- Ping the VLAN default gateway.

### Issue 2: Inter-VLAN Communication Failure

**Symptoms:**
- Devices in different VLANs cannot communicate when the security policy should permit it.

**Possible causes:**
- Incorrect router subinterface configuration.
- Missing 802.1Q encapsulation.
- Trunk link not operational.
- Incorrect default gateway.
- ACL denying the traffic.

**Troubleshooting commands:**

```cisco
show ip interface brief
show interfaces trunk
show ip route
show access-lists
```

**Corrective actions:**
- Verify router-on-a-stick subinterfaces.
- Check VLAN tags and trunk configuration.
- Confirm the correct gateway on each PC.
- Check whether ACLs intentionally block the traffic.

**Verification:**
- Ping the source VLAN gateway.
- Ping the destination VLAN gateway.
- Test communication between permitted VLANs.

### Issue 3: OSPF Neighbor Not Forming

**Symptoms:**
- OSPF neighbor does not appear.
- Remote networks are missing from the routing table.

**Possible causes:**
- Router interfaces are down.
- Incorrect IP addressing.
- Incorrect OSPF network statements.
- OSPF area mismatch.
- Incorrect passive-interface configuration.

**Troubleshooting commands:**

```cisco
show ip ospf neighbor
show ip protocols
show ip interface brief
show ip route ospf
```

**Corrective actions:**
- Verify R1 and ISP-R2 transit interfaces.
- Confirm both routers use OSPF Area 0.
- Check network statements.
- Ensure OSPF is enabled on the transit link.

**Verification:**
- Confirm OSPF neighbor state is FULL.
- Verify OSPF-learned routes.
- Ping the simulated external network address `203.0.113.1`.

### Issue 4: ACL Blocking or Allowing Incorrect Traffic

**Symptoms:**
- HR can communicate with Sales when it should be blocked.
- Permitted communication fails unexpectedly.

**Possible causes:**
- Incorrect source or destination network.
- ACL applied to the wrong interface.
- Incorrect ACL direction.
- Missing permit statement.
- Rule order incorrect.

**Troubleshooting commands:**

```cisco
show access-lists
show ip interface gigabitEthernet0/0.10
show ip interface gigabitEthernet0/0.30
```

**Corrective actions:**
- Verify HR_POLICY and SALES_POLICY.
- Confirm ACLs are applied inbound on the correct router subinterfaces.
- Check rule order.
- Ensure permitted traffic is not blocked by an implicit deny.

**Verification:**
- HR to Sales ping should fail.
- Sales to HR ping should fail.
- HR and Sales should be able to reach Server1.

### Issue 5: SSH Remote Management Failure

**Symptoms:**
- IT-PC1 cannot establish an SSH session.
- Unauthorized departments can access management services.

**Possible causes:**
- Incorrect management IP address.
- Missing RSA keys.
- SSH not enabled.
- Incorrect local authentication configuration.
- Incorrect VTY access restrictions.

**Troubleshooting commands:**

```cisco
show ip ssh
show access-lists
show running-config
show ip interface brief
```

**Corrective actions:**
- Verify the management IP and gateway.
- Confirm SSH version 2.
- Check local username authentication.
- Verify the IT_SSH_ONLY access list.
- Check the VTY line configuration.

**Verification:**

From IT-PC1:

```text
ssh -l netadmin 192.168.20.1
ssh -l netadmin 192.168.20.2
ssh -l netadmin 192.168.20.3
```

SSH should succeed with valid credentials.

SSH from HR and Sales should be denied.

### Issue 6: Port Security Violation

**Symptoms:**
- Switch access port becomes disabled.
- A connected device cannot communicate.

**Possible causes:**
- Unauthorized MAC address detected.
- Maximum secure MAC address limit exceeded.
- Sticky MAC address mismatch.

**Troubleshooting commands:**

```cisco
show port-security
show port-security address
show port-security interface fastEthernet0/1
```

**Corrective actions:**
- Identify the unauthorized device.
- Remove it from the secured port.
- Reconnect the original authorized device.
- Recover the interface if appropriate.

**Interface recovery:**

```cisco
configure terminal
interface fastEthernet0/1
shutdown
no shutdown
end
```

**Verification:**
- Confirm the port returns to a secure operational state.
- Verify that the authorized PC can communicate.

## 4. Troubleshooting Commands Summary

| Command | Purpose |
|---|---|
| `show ip interface brief` | Check router and switch interface status |
| `show vlan brief` | Verify VLAN assignments |
| `show interfaces trunk` | Verify trunk links |
| `show ip route` | Inspect routing table |
| `show ip ospf neighbor` | Verify OSPF adjacency |
| `show ip dhcp binding` | Verify DHCP leases |
| `show access-lists` | Inspect ACL rules and counters |
| `show ip ssh` | Verify SSH status |
| `show port-security` | Inspect port security status |
| `show port-security address` | Inspect learned secure MAC addresses |
| `show running-config` | Review current configuration |

## 5. Actual Troubleshooting Record

Use this section to document issues personally encountered during the lab.

| Date | Device | Issue Observed | Root Cause | Fix Applied | Retest Result |
|---|---|---|---|---|---|
| To be completed | — | — | — | — | — |

Do not report example problems as actual incidents unless they occurred during testing.

## 6. Conclusion

The troubleshooting process provides practice in identifying configuration errors, checking device states, analyzing routing and security policies, and validating corrections.

These techniques are relevant to network support, NOC operations, and entry-level network engineering.

**Project:** Enterprise Company Network Design  
**Platform:** Cisco Packet Tracer  
**Status:** Troubleshooting reference completed; actual incident log to be updated.
