# DNS Lab

Date: March 02, 2026
Topic: DNS Configuration
Stage: Stage 1 — Beginner
Status: ✅ Completed

## What I Built
Added a DNS server to existing network so PCs
can find websites by name instead of IP address.

## Network Layout
Router (2911)
      |
    Switch 1 (IT Department)
   /        \          \
PC0         PC1        Server
                       192.168.1.50

## Server Static IP
IP Address:      192.168.1.50
Subnet Mask:     255.255.255.192
Default Gateway: 192.168.1.1

## DNS Records Added
company.com     → 192.168.1.50
www.company.com → 192.168.1.50

## Commands Used

# Tell router about DNS server
ip name-server 192.168.1.50

# Update DHCP pools with DNS server
ip dhcp pool IT-DEPARTMENT
dns-server 192.168.1.50

ip dhcp pool OFFICE-STAFF
dns-server 192.168.1.50

ip dhcp pool MANAGEMENT
dns-server 192.168.1.50

# Verify name server
show running-config | include name-server

# Verify DHCP binding
show ip dhcp binding

# Test DNS from PC
ping company.com
nslookup company.com

## Test Results
- Server connected to Switch 1 ✅
- Server static IP configured ✅
- DNS service turned on ✅
- DNS records added ✅
- PC0 DNS server showing 192.168.1.50 ✅
- Web browser http://company.com tested ✅

## What I Learned
- DNS converts domain names to IP addresses
- DNS server always needs static IP
- Server connects to switch not router directly
- DHCP tells PCs which DNS server to use
- A Record converts name to IP address
- nslookup command tests DNS resolution
- Web browser can test DNS using domain name
