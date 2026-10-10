# Troubleshooting Report

## Issue #001 — Network Connectivity Problem

### Problem
ADMIN-PC1 could not communicate with the network gateway during the troubleshooting exercise.

### Initial Configuration
- IP Address: 192.168.10.10
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.10.1

### Investigation
The IP configuration was checked using:

ipconfig

Connectivity was tested using:

ping 192.168.10.1

### Root Cause
The cause was investigated by checking the PC's IP address, subnet mask, default gateway, and physical network connection.

### Solution
The incorrect network setting was corrected, and the original configuration was restored.

### Verification
Connectivity was tested again using:

ping 192.168.10.1

Result: Successful connectivity with 0% packet loss.

### Lesson Learned
Correct IP addressing, subnet masks, default gateways, and physical connections are essential for reliable network communication.
