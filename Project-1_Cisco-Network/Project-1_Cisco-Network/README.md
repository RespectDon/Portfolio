# Cisco Packet Tracer – Home Network

## Overview

This project demonstrates a basic home network built in Cisco Packet Tracer. It includes two LAN segments connected by a router, with four PCs distributed across the two LANs. Static IP addresses were used for each device, and connectivity was verified using ICMP (ping) and PDU tools.

## Topology

Devices used:
- 1 Router (1941)
- 2 Switches (2960-24TT)
- 4 PCs (PC0–PC3)

Network structure:
- LAN 1 (192.168.1.0/24): PC0 and PC1
- LAN 2 (192.168.2.0/24): PC2 and PC3
- Router interfaces:
  - Gig0/0: 192.168.1.1
  - Gig0/1: 192.168.2.1

## Configuration

### Router Commands
```bash
enable
configure terminal
interface gig0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
interface gig0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
