# Lab 07 — Inter-VLAN Routing
Date: March 13, 2026
Topic: Inter-VLAN Routing (Router on a Stick)
Stage: Stage 2 — Intermediate
Status: ✅ Completed

---

## What I Built
A company network with 1 router and 1 switch.
3 departments separated by VLANs.
Router controls communication between VLANs.
Using Router on a Stick method.
One cable between switch and router
carries all VLAN traffic!


---

## Network Layout
Router (2911)
      |
    Switch
   /   |   \
 PC0  PC1  PC2
VLAN10 VLAN20 VLAN30
IT    Office  Management

---

## IP Plan

VLAN 10 - IT Department
Router subinterface = 192.168.10.1
PC0 = 192.168.10.10
Subnet Mask = 255.255.255.0
Gateway = 192.168.10.1

VLAN 20 - Office Staff
Router subinterface = 192.168.20.1
PC1 = 192.168.20.10
Subnet Mask = 255.255.255.0
Gateway = 192.168.20.1

VLAN 30 - Management
Router subinterface = 192.168.30.1
PC2 = 192.168.30.10
Subnet Mask = 255.255.255.0
Gateway = 192.168.30.1

---

## Cable Types
PC to Switch     = Copper Straight-Through
Switch to Router = Copper Straight-Through
                   configured as trunk port
                   carries all VLAN traffic!

---

## Step 1 - Created VLANs on Switch

What I did:
Created 3 VLANs on the switch.
Each VLAN represents one department.
VLANs separate departments so they
cannot talk to each other directly.

Commands:
en
conf t
vlan 10
name IT
exit
vlan 20
name OFFICE
exit
vlan 30
name MANAGEMENT
exit

Why:
Without VLANs all PCs on same switch
can see each others traffic.
VLANs create invisible walls between
departments for security!

---

## Step 2 - Set Trunk Port

What I did:
Set Fa0/1 as trunk port between
switch and router.

Commands:
interface fa0/1
switchport mode trunk
exit

Why:
Trunk port carries ALL VLAN traffic
through one single cable to router.
This is the Router on a Stick concept!
Without trunk port VLANs cannot
reach the router at all!

---

## Step 3 - Set Access Ports

What I did:
Assigned each PC port to correct VLAN.
Each port can only carry one VLAN traffic.

Commands:
interface fa1/1
switchport mode access
switchport access vlan 10
exit
interface fa2/1
switchport mode access
switchport access vlan 20
exit
interface fa3/1
switchport mode access
switchport access vlan 30
exit

Why:
Access ports tell the switch which
VLAN each PC belongs to.
PC0 on fa1/1 = VLAN 10 (IT)
PC1 on fa2/1 = VLAN 20 (Office)
PC2 on fa3/1 = VLAN 30 (Management)

---

## Step 4 - Verified Switch

Commands:
show vlan brief
show interfaces trunk

What I checked:
VLAN 10 IT active on fa1/1
VLAN 20 OFFICE active on fa2/1
VLAN 30 MANAGEMENT active on fa3/1
Fa0/1 showing as trunking
VLANs 1,10,20,30 allowed on trunk

---

## Step 5 - Configured Router

What I did:
First turned on physical interface.
Then created 3 subinterfaces.
Each subinterface handles one VLAN.

Why turn on physical interface first?
Cisco router ports are OFF by default!
Subinterfaces cannot work if physical
interface is down!

Commands:
en
conf t
interface gig0/0
no shutdown
exit

Subinterface for VLAN 10:
interface gig0/0.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0
exit

Subinterface for VLAN 20:
interface gig0/0.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0
exit

Subinterface for VLAN 30:
interface gig0/0.30
encapsulation dot1q 30
ip address 192.168.30.1 255.255.255.0
exit

What each command does:
interface gig0/0.10 = create subinterface
encapsulation dot1q 10 = tag traffic as VLAN 10
ip address = this becomes gateway for VLAN 10

---

## Step 6 - Verified Router

Command:
show ip interface brief

Result:
Gig0/0        unassigned  up  up
Gig0/0.10     192.168.10.1 up up ✅
Gig0/0.20     192.168.20.1 up up ✅
Gig0/0.30     192.168.30.1 up up ✅

---

## Step 7 - Configured PCs

Each PC needs 3 settings:
1. IP Address  = unique address of that PC
2. Subnet Mask = size of the network
3. Gateway     = router subinterface IP

How to configure in Packet Tracer:
1. Click on PC
2. Click Desktop tab
3. Click IP Configuration
4. Type IP, Mask and Gateway
5. Close window

PC0 settings:
IP:      192.168.10.10
Mask:    255.255.255.0
Gateway: 192.168.10.1

PC1 settings:
IP:      192.168.20.10
Mask:    255.255.255.0
Gateway: 192.168.20.1

PC2 settings:
IP:      192.168.30.10
Mask:    255.255.255.0
Gateway: 192.168.30.1

---

## Step 8 - Tested Connectivity

How data travels from PC0 to PC1:
1. PC0 sends data to gateway 192.168.10.1
2. Data goes through trunk port to router
3. Router receives data tagged as VLAN 10
4. Router routes data to VLAN 20
5. Router sends out Gig0/0.20
6. Switch delivers to PC1 in VLAN 20
7. Success! ✅

Test commands on PC0:
ping 192.168.20.10 = VLAN10 to VLAN20 ✅
ping 192.168.30.10 = VLAN10 to VLAN30 ✅

---

## Verification Commands
show vlan brief          = check VLANs on switch
show interfaces trunk    = check trunk port
show ip interface brief  = check subinterfaces
show ip route            = check routing table

---

## Important Terms
VLAN         = virtual wall separating departments
Trunk port   = carries all VLAN traffic
Access port  = carries one VLAN only
Subinterface = virtual router port per VLAN
dot1q        = tags each packet with VLAN ID
Encapsulation= wrapping data with VLAN tag
Router on a Stick = one cable all VLANs
Gateway      = router subinterface IP per VLAN

---

## Things to Remember
1. Always turn on physical interface first
   before creating subinterfaces!

2. encapsulation dot1q number must match
   the VLAN number exactly!
   dot1q 10 goes with VLAN 10 only!

3. Trunk port always between Switch and Router!

4. Access port always between Switch and PC!

5. Each subinterface needs different subnet!
   VLAN 10 = 192.168.10.x
   VLAN 20 = 192.168.20.x
   VLAN 30 = 192.168.30.x

---

## What I Learned
- VLANs separate departments for security
- Without VLANs all PCs see each others traffic
- Trunk port carries all VLAN traffic on one cable
- Access port carries only one VLAN traffic
- Router on a Stick uses one cable for all VLANs
- Subinterfaces handle each VLAN on router
- dot1q tags each packet with correct VLAN ID
- Must turn on physical interface before subinterfaces
- Each VLAN needs its own subnet and gateway
- Router controls which VLANs can communicate

---

## Real World Use
Every real company uses VLANs and
Inter-VLAN Routing for security.

Hospital example:
Patient records = VLAN 10 (protected)
Doctor systems  = VLAN 20 (restricted)
Admin office    = VLAN 30 (limited)
Router controls who can access what!

School example:
Students  = VLAN 10 (limited internet)
Teachers  = VLAN 20 (more access)
Principal = VLAN 30 (full access)

Company example:
IT Department = VLAN 10
Finance       = VLAN 20 (protected data)
Management    = VLAN 30 (confidential)

---

## Simple Summary
VLAN alone    = departments separated
Inter-VLAN    = router allows communication
Trunk port    = highway for all VLANs
Access port   = single road one VLAN
Subinterface  = virtual port per VLAN
dot1q         = labels packet with VLAN ID
Router on stick = one cable all VLANs

---
Next Topic: Day 11 — NAT
Previous Topic: Day 10 — Inter-VLAN Routing Theory
