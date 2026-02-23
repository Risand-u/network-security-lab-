# network-security-lab 🔐
 
## Overview
A simulated network security lab built using 
Cisco Packet Tracer demonstrating real world 
network security concepts.

## Network Topology
- Cisco 2911 Router
- Cisco Switch
- 2 PCs

## What I Implemented
- Configured IP addresses on all devices
- Connected devices using proper cable types
- Set up VLANs to separate Office and Guest networks
- Created ACL Firewall rules to block unauthorized access
- Tested connectivity using ping commands

## Tools Used
- Cisco Packet Tracer
- Cisco 2911 Router
- Cisco IOS CLI

## Key Commands Used
ip address 192.168.1.1 255.255.255.0
no shutdown
access-list 100 deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
access-list 100 permit ip any any

## Screenshots
(screenshots are in the screenshots folder)

## What I Learned
- How routers and switches work together
- How to configure IP addresses using Cisco CLI
- How VLANs separate network traffic
- How ACL rules work as a basic firewall
- Difference between FastEthernet and GigabitEthernet

## Disclaimer
This project was built in an isolated virtual 
lab environment for educational purposes only.
