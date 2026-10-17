# DHCP and DNS Configuration in a Small Office Network

## 1. Project Overview

This project demonstrates the configuration of Dynamic Host Configuration Protocol (DHCP) and Domain Name System (DNS) services in a small office network using Cisco Packet Tracer.

DHCP automatically assigns IP configuration to client devices, while DNS allows clients to resolve domain names into IP addresses.

The network uses a router as the default gateway, a switch to connect devices, and a server to provide DHCP and DNS services.

## 2. Project Objectives

- Design a small office network using Cisco Packet Tracer.
- Configure IPv4 addressing for network devices.
- Configure DHCP to assign IP addresses automatically.
- Configure DNS to resolve a test hostname.
- Verify connectivity between clients, the server, and the router.
- Document configuration steps and troubleshooting results.

## 3. Network Topology

**Topology:** To be added after building the network in Cisco Packet Tracer.

The planned network contains:

- 1 router
- 1 Layer 2 switch
- 1 server
- 2 client PCs

## 4. IP Addressing Plan

| Device | IP address | Configuration |
|---|---|---|
| Router (R1) | 192.168.10.1/24 | Static |
| Server (Server1) | 192.168.10.10/24 | Static |
| Switch (SW1) | To be decided | Optional management IP |
| PC1 | DHCP assigned | Automatic |
| PC2 | DHCP assigned | Automatic |

### Planned DHCP Settings

- Network: `192.168.10.0/24`
- Address pool: `192.168.10.100`–`192.168.10.149`
- Default gateway: `192.168.10.1`
- DNS server: `192.168.10.10`

### Planned DNS Record

- Hostname: `intranet.office.test`
- Address: `192.168.10.10`

These are the proposed settings and will be updated if the final implementation differs.

## 5. Technologies Used

- Cisco Packet Tracer
- IPv4 addressing and subnetting
- DHCP
- DNS
- Ethernet switching and routing
- Basic network troubleshooting

## 6. Implementation

This section will document the configuration of the router, switch, server, DHCP service, DNS service, and client devices after implementation.

## 7. Testing and Verification

The following tests will be performed after configuration:

- Verify that both PCs receive valid IP addresses through DHCP.
- Verify the subnet mask, default gateway, and DNS server on each PC.
- Test connectivity to the default gateway.
- Test connectivity to the server.
- Verify that the DNS hostname resolves to the expected IP address.

Actual results and screenshots will be added after testing.

## 8. Repository Structure

```text
03-dhcp-dns-small-office/
├── README.md
├── configurations/
├── documentation/
├── packet-tracer/
└── screenshots/
```

## 9. Learning Outcomes

This section will be completed after the lab, describing the networking concepts learned and the troubleshooting challenges encountered.

## 10. Future Improvements

- Add VLAN segmentation.
- Introduce ACL-based traffic restrictions.
- Extend the topology with additional office clients.
- Explore network monitoring and logging.
