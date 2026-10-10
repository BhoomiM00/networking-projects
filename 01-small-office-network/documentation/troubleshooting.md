# Troubleshooting Report

## Issue #001 — ADMIN-PC1 Cannot Reach Default Gateway

### Problem

ADMIN-PC1 was unable to communicate with the network gateway.

### Initial Configuration

IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.254

### Investigation

The IP configuration was checked using:

ipconfig

Connectivity was tested using:

ping 192.168.10.1

### Root Cause

The default gateway configured on ADMIN-PC1 was incorrect.

The actual router gateway is:

192.168.10.1

### Solution

The default gateway was changed from:

192.168.10.254

to:

192.168.10.1

### Verification

After correcting the gateway, connectivity was tested again using:

ping 192.168.10.1

The connection was successful with 0% packet loss.

### Lesson Learned

Incorrect IP configuration, including an incorrect default gateway, can prevent a host from communicating correctly with other networks and network devices.
