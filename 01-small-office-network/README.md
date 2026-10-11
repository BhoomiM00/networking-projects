# Small Office Network — Cisco Packet Tracer

## 1. Project Overview

This project demonstrates the design, configuration, and testing of a basic small-office network using Cisco Packet Tracer.

The simulated network connects six computers to a Cisco switch, which connects to a Cisco router. Static IPv4 addressing is used to enable communication between devices on the same LAN.

## 2. Objectives

- Build a basic office LAN topology.
- Configure a router interface and a switch management interface.
- Assign static IPv4 addresses to end devices.
- Configure default gateways.
- Verify network connectivity using ping.
- Practice basic device security and troubleshooting.
- Document configurations and test results.

## 3. Network Topology

The network consists of one router, one switch, and six PCs.

**Devices:**
- 1 × Cisco 2911 Router (R1)
- 1 × Cisco 2960 Switch (SW1)
- 6 × PCs

**Departments represented:**
- Administration: ADMIN-PC1, ADMIN-PC2
- Human Resources: HR-PC1, HR-PC2
- IT: IT-PC1, IT-PC2

All PCs belong to the same LAN. The department names are used for identification; VLAN segmentation is not implemented in this project.

## 4. IP Addressing Plan

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| R1 G0/0 | 192.168.10.1 | 255.255.255.0 | — |
| SW1 VLAN 1 | 192.168.10.2 | 255.255.255.0 | 192.168.10.1 |
| ADMIN-PC1 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| ADMIN-PC2 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| HR-PC1 | 192.168.10.20 | 255.255.255.0 | 192.168.10.1 |
| HR-PC2 | 192.168.10.21 | 255.255.255.0 | 192.168.10.1 |
| IT-PC1 | 192.168.10.30 | 255.255.255.0 | 192.168.10.1 |
| IT-PC2 | 192.168.10.31 | 255.255.255.0 | 192.168.10.1 |

## 5. Configuration Highlights

### Router R1

- Set the hostname.
- Configured GigabitEthernet0/0 with `192.168.10.1/24`.
- Enabled the interface using `no shutdown`.
- Configured basic password protection and a warning banner.

### Switch SW1

- Set the hostname.
- Configured VLAN 1 with management IP `192.168.10.2/24`.
- Set the default gateway to `192.168.10.1`.
- Configured basic password protection and a warning banner.

### PCs

- Assigned static IPv4 addresses.
- Configured subnet masks and default gateways.

## 6. Verification and Testing

Connectivity was tested using the `ping` command.

Example:

```text
ping 192.168.10.1
```

Successful ping results indicate connectivity to the router interface.

Screenshots of the topology, device verification, and connectivity tests are stored in the `screenshots/` directory.

## 7. Troubleshooting

The lab includes troubleshooting documentation covering network configuration checks, connectivity testing, identifying possible causes, applying corrections, and verifying the result.

See [`documentation/troubleshooting.md`](documentation/troubleshooting.md).

## 8. Project Files

- `packet-tracer/` — Cisco Packet Tracer project file.
- `configurations/` — Router and switch running configurations.
- `screenshots/` — Topology and verification screenshots.
- `documentation/` — Troubleshooting report and lab notes.

## 9. Skills Practiced

- IPv4 addressing and subnet masks
- Basic router configuration
- Basic switch configuration
- Switch management SVI
- Default gateway configuration
- Cisco IOS command-line interface
- Ping-based connectivity testing
- Basic device access security
- Network documentation

## 10. Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- GitHub

## 11. Future Improvements

- Configure DHCP for automatic IP assignment.
- Implement VLANs and inter-VLAN routing in a separate lab.
- Add additional networks and routing protocols in future projects.

---

**Project status:** Completed basic LAN configuration and connectivity testing.
