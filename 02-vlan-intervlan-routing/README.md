# VLAN Configuration & Inter-VLAN Routing

## Project Overview

This project demonstrates how to segment a small company network using Virtual Local Area Networks (VLANs) and enable communication between them using router-on-a-stick inter-VLAN routing.

The network will be designed and configured in Cisco Packet Tracer.

## Objectives

- Create and configure VLANs.
- Assign switch access ports to the correct VLANs.
- Configure an 802.1Q trunk between the switch and router.
- Configure router subinterfaces for inter-VLAN routing.
- Assign IPv4 addresses to PCs.
- Verify connectivity within and between VLANs.
- Document configurations and troubleshooting results.

## Network Devices

| Device | Model | Purpose |
|---|---|---|
| R1 | Cisco 2911 | Inter-VLAN routing |
| SW1 | Cisco 2960 | VLAN and switch-port configuration |
| 4 PCs | End devices | Connectivity testing |

## VLAN and IP Addressing Plan

| Department | VLAN ID | Network | Default Gateway |
|---|---:|---|---|
| Administration | 10 | 192.168.10.0/24 | 192.168.10.1 |
| IT | 20 | 192.168.20.0/24 | 192.168.20.1 |

## Planned Configuration

- Create VLAN 10 and VLAN 20 on SW1.
- Assign two PCs to each VLAN.
- Configure a trunk link between SW1 and R1.
- Configure router subinterfaces using 802.1Q encapsulation.
- Test communication within each VLAN and between VLANs.

## Repository Contents

- `configurations/` — Router and switch configuration files.
- `documentation/` — Configuration notes and troubleshooting report.
- `packet-tracer/` — Cisco Packet Tracer project file.
- `screenshots/` — Configuration and connectivity evidence.

## Tools

- Cisco Packet Tracer
- Cisco IOS CLI
- GitHub

## Skills Practiced

- VLAN configuration
- Access and trunk ports
- 802.1Q encapsulation
- Router subinterfaces
- Inter-VLAN routing
- IPv4 addressing
- Network connectivity testing
- Troubleshooting and documentation

## Project Status

Completed — VLANs 10 and 20 were configured, an 802.1Q trunk was established, router-on-a-stick inter-VLAN routing was implemented, and connectivity was verified using ping.
