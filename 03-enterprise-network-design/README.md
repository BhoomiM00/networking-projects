# Enterprise Company Network Design — Cisco Packet Tracer

## 1. Project Overview

This project demonstrates the design, configuration, security, and testing of a simulated enterprise network using Cisco Packet Tracer.

The network represents a company with three departments — Human Resources (HR), Information Technology (IT), and Sales — along with a dedicated server network.

The project implements VLAN segmentation, inter-VLAN routing, DHCP, dynamic routing using OSPF, Access Control Lists (ACLs), SSH-based remote device management, and switch port security.

The objective is to create a functional, organized, and secure enterprise network while gaining practical experience with Cisco networking technologies.

## 2. Objectives

- Design and implement an enterprise network topology.
- Configure multiple VLANs for departmental network segmentation.
- Configure access ports and IEEE 802.1Q trunk links.
- Implement inter-VLAN routing using router-on-a-stick.
- Configure DHCP for automatic IP address allocation.
- Configure single-area OSPF for dynamic routing between routers.
- Implement extended ACLs to restrict communication between departments.
- Configure secure remote device management using SSH.
- Restrict SSH management access to the IT department.
- Configure switch port security using sticky MAC addresses.
- Implement port security violation detection and recovery.
- Disable unused switch ports to improve network security.
- Verify network connectivity and security policies.
- Troubleshoot network configuration and connectivity issues.
- Document configurations, network topology, and test results.

## 3. Network Topology

The enterprise network consists of two routers, two switches, six PCs, and one server.

**Devices:**

- 2 × Cisco 2911 Routers (R1 and ISP-R2)
- 2 × Cisco 2960-24TT Switches (SW1-Core and SW2-Access)
- 6 × PCs
- 1 × Server (Server1)

**Departments represented:**

- Human Resources: HR-PC1, HR-PC2
- Information Technology: IT-PC1, IT-PC2
- Sales: Sales-PC1, Sales-PC2
- Servers: Server1

### Logical Network Topology

```text
                   ISP-R2
              G0/0: 10.0.0.2/30
              Lo0: 203.0.113.1/24
                      |
                 OSPF Area 0
                      |
               G0/1: 10.0.0.1/30
                     R1
               Cisco 2911 Router
                      |
                 G0/0 Trunk
                      |
                  SW1-Core
                Cisco 2960
                  /       \
                 /         \
             Server1      Trunk
          192.168.40.10      |
           VLAN 40        SW2-Access
                          Cisco 2960
                              |
                 +------------+------------+
                 |            |            |
               VLAN 10      VLAN 20      VLAN 30
                 HR           IT          SALES
                 |            |            |
              HR-PC1       IT-PC1       Sales-PC1
              HR-PC2       IT-PC2       Sales-PC2
```

### Physical Connections

| Source Device | Interface | Destination Device | Interface |
|---|---|---|---|
| R1 | G0/1 | ISP-R2 | G0/0 |
| R1 | G0/0 | SW1-Core | G0/1 |
| SW1-Core | G0/2 | SW2-Access | G0/1 |
| SW1-Core | Fa0/2 | Server1 | Fa0 |
| SW2-Access | Fa0/1 | HR-PC1 | Fa0 |
| SW2-Access | Fa0/2 | HR-PC2 | Fa0 |
| SW2-Access | Fa0/3 | IT-PC1 | Fa0 |
| SW2-Access | Fa0/4 | IT-PC2 | Fa0 |
| SW2-Access | Fa0/5 | Sales-PC1 | Fa0 |
| SW2-Access | Fa0/6 | Sales-PC2 | Fa0 |

## 4. IP Addressing Plan

### VLAN Addressing

| VLAN ID | Department | Network Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| 10 | HR | 192.168.10.0/24 | 255.255.255.0 | 192.168.10.1 |
| 20 | IT | 192.168.20.0/24 | 255.255.255.0 | 192.168.20.1 |
| 30 | Sales | 192.168.30.0/24 | 255.255.255.0 | 192.168.30.1 |
| 40 | Servers | 192.168.40.0/24 | 255.255.255.0 | 192.168.40.1 |
| 999 | Unused Ports | No IP subnet assigned | — | — |

### Router and Switch IP Addresses

| Device | Interface | IP Address | Subnet Mask |
|---|---|---|---|
| R1 | G0/0.10 | 192.168.10.1 | 255.255.255.0 |
| R1 | G0/0.20 | 192.168.20.1 | 255.255.255.0 |
| R1 | G0/0.30 | 192.168.30.1 | 255.255.255.0 |
| R1 | G0/0.40 | 192.168.40.1 | 255.255.255.0 |
| R1 | G0/1 | 10.0.0.1 | 255.255.255.252 |
| ISP-R2 | G0/0 | 10.0.0.2 | 255.255.255.252 |
| ISP-R2 | Loopback0 | 203.0.113.1 | 255.255.255.0 |
| SW1-Core | VLAN 20 | 192.168.20.2 | 255.255.255.0 |
| SW2-Access | VLAN 20 | 192.168.20.3 | 255.255.255.0 |
| Server1 | Fa0 | 192.168.40.10 | 255.255.255.0 |

### DHCP Address Allocation

DHCP is configured on R1 to automatically assign IP addresses to HR, IT, and Sales PCs.

| Department | DHCP Pool | Available IP Range |
|---|---|---|
| HR | HR_POOL | 192.168.10.21–192.168.10.254 |
| IT | IT_POOL | 192.168.20.21–192.168.20.254 |
| Sales | SALES_POOL | 192.168.30.21–192.168.30.254 |

Addresses `.1` through `.20` are excluded from DHCP allocation in each employee VLAN.

Server1 uses a static IP address.

## 5. Configuration Highlights

### 5.1 VLAN Configuration

VLANs were created on both switches to separate departmental traffic.

**Cisco IOS commands:**

```cisco
enable
configure terminal

vlan 10
name HR
exit

vlan 20
name IT
exit

vlan 30
name SALES
exit

vlan 40
name SERVERS
exit

vlan 999
name UNUSED_PORTS
exit

end
```

**Verification:**

```cisco
show vlan brief
```

### 5.2 Access Port Configuration

Departmental access ports were configured on SW2-Access.

**HR — VLAN 10**

```cisco
configure terminal

interface range fastEthernet0/1 - 2
switchport mode access
switchport access vlan 10
exit
```

**IT — VLAN 20**

```cisco
interface range fastEthernet0/3 - 4
switchport mode access
switchport access vlan 20
exit
```

**Sales — VLAN 30**

```cisco
interface range fastEthernet0/5 - 6
switchport mode access
switchport access vlan 30
exit
```

**Server — VLAN 40 (SW1-Core)**

```cisco
interface fastEthernet0/2
switchport mode access
switchport access vlan 40
exit
```

### 5.3 Trunk Configuration

IEEE 802.1Q trunking was configured between R1 and SW1-Core and between SW1-Core and SW2-Access.

**SW1-Core:**

```cisco
configure terminal

interface gigabitEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
exit

interface gigabitEthernet0/2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
exit
```

**SW2-Access:**

```cisco
configure terminal

interface gigabitEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
exit
```

**Verification:**

```cisco
show interfaces trunk
```

### 5.4 Inter-VLAN Routing — Router-on-a-Stick

Inter-VLAN routing was implemented using subinterfaces on R1.

```cisco
enable
configure terminal

interface gigabitEthernet0/0
no shutdown
exit

interface gigabitEthernet0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface gigabitEthernet0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

interface gigabitEthernet0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit

interface gigabitEthernet0/0.40
encapsulation dot1Q 40
ip address 192.168.40.1 255.255.255.0
exit
```

Each subinterface acts as the default gateway for its corresponding VLAN.

**Verification:**

```cisco
show ip interface brief
show ip route
```

### 5.5 DHCP Configuration

R1 was configured as a DHCP server for the three employee departments.

```cisco
configure terminal

ip dhcp excluded-address 192.168.10.1 192.168.10.20
ip dhcp excluded-address 192.168.20.1 192.168.20.20
ip dhcp excluded-address 192.168.30.1 192.168.30.20

ip dhcp pool HR_POOL
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
exit

ip dhcp pool IT_POOL
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 8.8.8.8
exit

ip dhcp pool SALES_POOL
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 8.8.8.8
exit
```

All six PCs were configured to obtain IP addresses automatically.

**Verification:**

```cisco
show ip dhcp binding
show ip dhcp pool
```

### 5.6 OSPF Dynamic Routing

Single-area OSPFv2 was configured between R1 and ISP-R2.

**R1 transit interface:**

```cisco
configure terminal

interface gigabitEthernet0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
exit
```

**R1 OSPF configuration:**

```cisco
router ospf 1
router-id 1.1.1.1

network 10.0.0.0 0.0.0.3 area 0
network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0
network 192.168.40.0 0.0.0.255 area 0

passive-interface default
no passive-interface gigabitEthernet0/1
exit
```

**ISP-R2 interface configuration:**

```cisco
configure terminal

interface gigabitEthernet0/0
ip address 10.0.0.2 255.255.255.252
no shutdown
exit

interface loopback0
ip address 203.0.113.1 255.255.255.0
exit
```

**ISP-R2 OSPF configuration:**

```cisco
router ospf 1
router-id 2.2.2.2

network 10.0.0.0 0.0.0.3 area 0
network 203.0.113.0 0.0.0.255 area 0

passive-interface default
no passive-interface gigabitEthernet0/0
exit
```

**Verification:**

```cisco
show ip ospf neighbor
show ip route ospf
show ip protocols
```

Expected OSPF neighbor state: `FULL`.

The address `203.0.113.1` represents a simulated external network endpoint, not actual Internet connectivity.

### 5.7 Access Control Lists (ACLs)

Extended ACLs were configured on R1 to restrict communication between HR and Sales.

**HR Policy:**

```cisco
configure terminal

ip access-list extended HR_POLICY
remark Block HR access to Sales
deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
permit ip 192.168.10.0 0.0.0.255 any
exit

interface gigabitEthernet0/0.10
ip access-group HR_POLICY in
exit
```

**Sales Policy:**

```cisco
ip access-list extended SALES_POLICY
remark Block Sales access to HR
deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
permit ip 192.168.30.0 0.0.0.255 any
exit

interface gigabitEthernet0/0.30
ip access-group SALES_POLICY in
exit
```

**Access policy:**

| Source | Destination | Access |
|---|---|---|
| HR | Sales | Denied |
| Sales | HR | Denied |
| HR | Servers | Allowed |
| Sales | Servers | Allowed |
| IT | HR | Allowed |
| IT | Sales | Allowed |
| IT | Servers | Allowed |

**Verification:**

```cisco
show access-lists
show ip interface gigabitEthernet0/0.10
show ip interface gigabitEthernet0/0.30
```

### 5.8 SSH Configuration

SSH was configured to provide encrypted remote management of network devices.

**R1 SSH configuration:**

```cisco
configure terminal

ip domain-name enterprise.lab

username netadmin privilege 15 secret <YOUR_LAB_PASSWORD>

crypto key generate rsa
```

An RSA key size of 2048 bits was selected where supported by the Packet Tracer device image.

```cisco
ip ssh version 2

ip access-list standard IT_SSH_ONLY
permit 192.168.20.0 0.0.0.255
deny any
exit

line vty 0 4
login local
transport input ssh
access-class IT_SSH_ONLY in
exit
```

The same SSH access policy was configured on both switches.

**Switch management interfaces:**

SW1-Core:

```cisco
interface vlan 20
ip address 192.168.20.2 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.20.1
```

SW2-Access:

```cisco
interface vlan 20
ip address 192.168.20.3 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.20.1
```

The switches also use local authentication, RSA keys, SSH version 2, and the `IT_SSH_ONLY` access policy on their VTY lines.

**SSH testing from IT-PC1:**

```text
ssh -l netadmin 192.168.20.1
ssh -l netadmin 192.168.20.2
ssh -l netadmin 192.168.20.3
```

SSH access is restricted to the IT subnet.

**Verification:**

```cisco
show ip ssh
show access-lists
show running-config
```

**Security note:** The password placeholder is intentionally not a real credential. Do not publish actual device passwords, private keys, or sensitive configuration data in a public GitHub repository.

### 5.9 Switch Port Security

Port security was implemented on SW2-Access to restrict unauthorized devices from connecting to employee access ports.

**Configuration:**

```cisco
configure terminal

interface range fastEthernet0/1 - 6
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit
```

**Port security features:**

- Maximum one secure MAC address per access port.
- Automatic sticky MAC address learning.
- Shutdown violation mode.
- Unauthorized MAC address detection.
- Manual interface recovery.

**Verification:**

```cisco
show port-security
show port-security address
show port-security interface fastEthernet0/1
```

**Port recovery:**

```cisco
configure terminal

interface fastEthernet0/1
shutdown
no shutdown
exit
```

During testing, an unauthorized PC was connected to a secured port to demonstrate a port security violation.

The original PC was reconnected before the port was recovered.

### 5.10 Unused Port Security

Unused switch ports were assigned to VLAN 999 and administratively disabled.

**SW2-Access:**

```cisco
configure terminal

vlan 999
name UNUSED_PORTS
exit

interface range fastEthernet0/7 - 24
description UNUSED_DISABLED
switchport mode access
switchport access vlan 999
shutdown
exit

interface gigabitEthernet0/2
description UNUSED_DISABLED
switchport mode access
switchport access vlan 999
shutdown
exit
```

**SW1-Core:**

```cisco
configure terminal

vlan 999
name UNUSED_PORTS
exit

interface fastEthernet0/1
description UNUSED_DISABLED
switchport mode access
switchport access vlan 999
shutdown
exit

interface range fastEthernet0/3 - 24
description UNUSED_DISABLED
switchport mode access
switchport access vlan 999
shutdown
exit
```

This prevents unused switch interfaces from accepting unauthorized connections.

### 5.11 Saving Device Configurations

Configurations were saved using:

```cisco
copy running-config startup-config
```

This preserves the configurations after device reloads.

## 6. Verification and Testing

Network functionality and security were verified using Cisco IOS commands and end-device connectivity tests.

### Router Verification Commands

```cisco
show ip interface brief
show ip route
show ip ospf neighbor
show ip route ospf
show ip dhcp binding
show ip dhcp pool
show access-lists
show ip ssh
show running-config
```

### Switch Verification Commands

```cisco
show vlan brief
show interfaces trunk
show ip interface brief
show port-security
show port-security address
show interfaces status
show running-config
```

### Connectivity Test Cases

| Test | Expected Result |
|---|---|
| HR-PC1 → HR-PC2 | Successful |
| HR-PC1 → IT-PC1 | Successful |
| HR-PC1 → Sales-PC1 | Blocked |
| Sales-PC1 → HR-PC1 | Blocked |
| IT-PC1 → Sales-PC1 | Successful |
| HR-PC1 → Server1 | Successful |
| Sales-PC1 → Server1 | Successful |
| IT-PC1 → Server1 | Successful |
| IT-PC1 → 203.0.113.1 | Successful |
| IT-PC1 → R1 SSH | Successful |
| IT-PC1 → SW1-Core SSH | Successful |
| IT-PC1 → SW2-Access SSH | Successful |
| HR-PC1 → R1 SSH | Blocked |
| Sales-PC1 → SW1-Core SSH | Blocked |
| Unauthorized PC → Secured port | Port security violation |

**Example ping commands:**

```text
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.40.10
ping 203.0.113.1
```

**Example SSH command:**

```text
ssh -l netadmin 192.168.20.1
```

Test outcomes should be recorded based on actual Packet Tracer results.

Screenshots of the network topology, VLANs, DHCP bindings, OSPF neighbors, ACL testing, SSH connectivity, and port security are stored in the `screenshots/` directory.

## 7. Troubleshooting

The project includes troubleshooting exercises involving addressing, routing, switching, and network security.

### Common Issues and Solutions

| Problem | Possible Cause | Troubleshooting |
|---|---|---|
| PC not receiving IP address | DHCP or VLAN configuration issue | Verify DHCP pool and access VLAN |
| Inter-VLAN communication fails | Router subinterface or trunk issue | Verify 802.1Q encapsulation and trunk |
| OSPF neighbor not forming | Incorrect OSPF or interface configuration | Check neighbor state and OSPF network statements |
| Department access policy not working | Incorrect ACL or direction | Verify ACL rules and interface application |
| SSH connection fails | VTY, management IP, or RSA key issue | Check SSH configuration and access policy |
| Switch port becomes disabled | Port security violation | Check learned MAC and recover interface |
| Server unreachable | VLAN, gateway, or ACL issue | Verify server addressing and routing |
| External network unreachable | Missing OSPF route | Verify routing tables on both routers |

### Troubleshooting Commands

```cisco
show ip interface brief
show vlan brief
show interfaces trunk
show ip route
show ip ospf neighbor
show ip dhcp binding
show access-lists
show port-security
show port-security address
show running-config
```

### Troubleshooting Methodology

1. Identify the connectivity or security issue.
2. Check physical connections and interface status.
3. Verify IP addressing and subnet configuration.
4. Check VLAN assignments and trunk links.
5. Inspect routing tables and OSPF neighbors.
6. Verify ACL rules and port security settings.
7. Apply the necessary correction.
8. Repeat connectivity and security tests.
9. Document the issue and its resolution.

## 8. Project Files

The GitHub repository is organized as follows:

```text
Enterprise-Company-Network/
|
|-- README.md
|
|-- packet-tracer/
|   |-- Enterprise_Company_Network_Final.pkt
|
|-- configurations/
|   |-- R1.txt
|   |-- ISP-R2.txt
|   |-- SW1-Core.txt
|   |-- SW2-Access.txt
|
|-- screenshots/
|   |-- network-topology.png
|   |-- vlan-configuration.png
|   |-- trunk-verification.png
|   |-- dhcp-bindings.png
|   |-- ospf-neighbor.png
|   |-- acl-verification.png
|   |-- ssh-testing.png
|   |-- port-security.png
|
|-- documentation/
|   |-- troubleshooting.md
|   |-- testing-results.md
```

**Directory descriptions:**

- `packet-tracer/` — Final Cisco Packet Tracer network simulation file.
- `configurations/` — Sanitized router and switch configuration files.
- `screenshots/` — Network topology and verification screenshots.
- `documentation/` — Troubleshooting notes and final testing results.
- `README.md` — Complete project overview and configuration documentation.

Only include files and folders that are actually present in the repository.

## 9. Skills Practiced

### Networking Fundamentals

- IPv4 addressing and subnetting
- Default gateway configuration
- Network topology design
- Enterprise network segmentation
- Cisco IOS CLI configuration

### Switching

- VLAN creation and management
- Access port configuration
- IEEE 802.1Q trunking
- Switch management SVIs
- MAC address learning
- Switch port security
- Sticky MAC address configuration
- Port security violation handling
- Unused port shutdown

### Routing

- Router-on-a-stick
- Inter-VLAN routing
- Connected and dynamic routes
- OSPFv2 single-area configuration
- OSPF neighbor verification
- Routing table analysis

### Network Services

- DHCP server configuration
- DHCP address exclusions
- DHCP pool management
- Static IP addressing
- DNS server assignment through DHCP

### Network Security

- Extended ACL configuration
- Departmental access restrictions
- SSH remote management
- VTY access restrictions
- Local username authentication
- RSA key generation
- Secure switch management
- Port security violation recovery

### Troubleshooting and Documentation

- Ping-based connectivity testing
- Cisco IOS verification commands
- Network troubleshooting methodology
- Configuration backup
- Technical documentation
- GitHub project organization

## 10. Tools Used

- **Cisco Packet Tracer** — Network simulation, configuration, and testing.
- **Cisco IOS CLI** — Router and switch configuration.
- **GitHub** — Project documentation and portfolio hosting.
- **Markdown** — Technical README documentation.

### Networking Technologies

- IPv4
- Ethernet
- VLANs
- IEEE 802.1Q
- DHCP
- OSPFv2
- Extended ACLs
- SSH
- Port Security

## 11. Future Improvements

The following features can be implemented to extend the project:

- Configure NAT/PAT for simulated Internet access.
- Implement IPv6 addressing and routing.
- Configure OSPF authentication where supported.
- Add redundant switches and router connections.
- Implement Rapid Spanning Tree Protocol (RSTP).
- Configure EtherChannel for link aggregation.
- Implement centralized AAA authentication.
- Add dedicated management VLANs.
- Configure DHCP snooping and Dynamic ARP Inspection.
- Implement SNMP-based network monitoring.
- Configure Syslog for centralized event logging.
- Add network monitoring dashboards.
- Implement additional server services such as DNS and HTTP.
- Expand the network with additional departments and branch offices.

---

## Project Summary

This project demonstrates the practical implementation of an enterprise network using Cisco Packet Tracer.

It combines switching, routing, DHCP, OSPF, network access control, SSH remote management, and switch port security into a single network simulation.

The project provides hands-on experience with concepts relevant to CCNA preparation and entry-level networking roles such as:

- Network Support Engineer
- Network Engineer Intern
- NOC Engineer Intern
- IT Infrastructure Intern
- Network Administrator Intern

**Project Status:** Configuration completed; final verification and documentation should reflect actual test results.
