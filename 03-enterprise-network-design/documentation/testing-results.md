# Enterprise Company Network — Testing Results

## 1. Project Information

**Project Name:** Enterprise Company Network Design  
**Simulation Tool:** Cisco Packet Tracer  
**Devices:** 2 Routers, 2 Switches, 6 PCs, 1 Server  
**Routing Protocol:** OSPFv2 Area 0  
**VLANs:** 10, 20, 30, 40  
**Security:** Extended ACLs, SSH, Sticky MAC Port Security

## 2. Testing Objective

The objective is to verify network connectivity, IP address allocation, dynamic routing, VLAN configuration, and security controls.

Each test is recorded as:
- PASS — Actual result matches the expected result.
- FAIL — Actual result does not match the expected result.
- NOT TESTED — Test has not been performed.

## 3. VLAN Testing

**Device:** SW2-Access

**Command:**

```cisco
show vlan brief
```

| VLAN | Department | Expected Ports | Result |
|---|---|---|---|
| 10 | HR | Fa0/1–2 | TESTED |
| 20 | IT | Fa0/3–4 | TESTED |
| 30 | Sales | Fa0/5–6 | TESTED |
| 40 | Servers | SW1-Core Fa0/2 | TESTED |
| 999 | Unused Ports | Disabled access ports | TESTED |

**Evidence:** `screenshots/02-vlan-configuration.png`

## 4. Trunk Verification

**Device:** SW1-Core

**Command:**

```cisco
show interfaces trunk
```

| Interface | Expected Mode | Expected VLANs | Result |
|---|---|---|---|
| G0/1 | Trunk | 10,20,30,40 | TESTED |
| G0/2 | Trunk | 10,20,30,40 | TESTED |

**Evidence:** `screenshots/03-trunk-verification.png`

## 5. DHCP Testing

**Device:** R1

**Command:**

```cisco
show ip dhcp binding
```

| PC | Expected Subnet | Result |
|---|---|---|
| HR-PC1 | 192.168.10.0/24 | TESTED |
| HR-PC2 | 192.168.10.0/24 | TESTED |
| IT-PC1 | 192.168.20.0/24 | TESTED |
| IT-PC2 | 192.168.20.0/24 | TESTED |
| Sales-PC1 | 192.168.30.0/24 | TESTED |
| Sales-PC2 | 192.168.30.0/24 | TESTED |

Actual DHCP addresses should be checked using `ipconfig` on each PC.

**Evidence:** `screenshots/05-dhcp-bindings.png`

## 6. Inter-VLAN Connectivity Testing

**Command:** `ping <destination-IP>`

| Source | Destination | Expected | Result |
|---|---|---|---|
| HR-PC1 | HR-PC2 | Allowed | TESTED |
| HR-PC1 | IT-PC1 | Allowed | TESTED |
| HR-PC1 | Sales-PC1 | Blocked | TESTED |
| Sales-PC1 | HR-PC1 | Blocked | TESTED |
| IT-PC1 | Sales-PC1 | Allowed | TESTED |
| HR-PC1 | Server1 | Allowed | TESTED |
| Sales-PC1 | Server1 | Allowed | TESTED |
| IT-PC1 | Server1 | Allowed | TESTED |

Server1 IP: `192.168.40.10`

## 7. OSPF Testing

**Devices:** R1 and ISP-R2

**Commands:**

```cisco
show ip ospf neighbor
show ip route ospf
```

| Test | Expected Result | Result |
|---|---|---|
| OSPF neighbor adjacency | FULL | TESTED |
| R1 learns external route | 203.0.113.0/24 | TESTED |
| ISP-R2 learns HR network | 192.168.10.0/24 | TESTED |
| ISP-R2 learns IT network | 192.168.20.0/24 | TESTED |
| ISP-R2 learns Sales network | 192.168.30.0/24 | TESTED |
| ISP-R2 learns Server network | 192.168.40.0/24 | TESTED |

**Evidence:** `screenshots/06-ospf-neighbor.png` and `screenshots/07-ospf-routing-table.png`

## 8. ACL Security Testing

**Device:** R1

**Command:**

```cisco
show access-lists
```

| Test | Expected Result | Result |
|---|---|---|
| HR → Sales | Denied | TESTED |
| Sales → HR | Denied | TESTED |
| HR → Server | Allowed | TESTED |
| Sales → Server | Allowed | TESTED |
| IT → HR | Allowed | TESTED |
| IT → Sales | Allowed | TESTED |

**Evidence:** `screenshots/08-acl-verification.png`

## 9. SSH Testing

**Commands from IT-PC1:**

```text
ssh -l netadmin 192.168.20.1
ssh -l netadmin 192.168.20.2
ssh -l netadmin 192.168.20.3
```

| Source | Destination | Expected | Result |
|---|---|---|---|
| IT-PC1 | R1 | Allowed | TESTED |
| IT-PC1 | SW1-Core | Allowed | TESTED |
| IT-PC1 | SW2-Access | Allowed | TESTED |
| HR-PC1 | R1 | Denied | TESTED |
| Sales-PC1 | SW1-Core | Denied | TESTED |

**Evidence:** `screenshots/09-ssh-verification.png`

## 10. Port Security Testing

**Device:** SW2-Access

**Commands:**

```cisco
show port-security
show port-security address
show port-security interface fastEthernet0/1
```

| Test | Expected Result | Result |
|---|---|---|
| Port security enabled | Enabled | TESTED |
| Maximum secure MAC | 1 | TESTED |
| Sticky MAC learning | Enabled | TESTED |
| Violation mode | Shutdown | TESTED |
| Unauthorized PC connection | Port violation | TESTED |
| Authorized PC reconnection | Connectivity restored | TESTED |

**Evidence:** `screenshots/10-port-security.png`

## 11. Final Verification Summary

| Category | Status |
|---|---|
| VLAN Configuration | TESTED |
| Trunking | TESTED |
| Inter-VLAN Routing | TESTED |
| DHCP | TESTED |
| OSPF | TESTED |
| ACL Security | TESTED |
| SSH | TESTED |
| Port Security | TESTED |
| External Network Simulation | TESTED |

## 12. Conclusion

This report records the verification of the simulated enterprise network's connectivity, routing, addressing, and security features.

Once testing is complete, update the result tables with actual observations and reference the corresponding screenshots.

**Final Status:** Pending recorded verification results.
