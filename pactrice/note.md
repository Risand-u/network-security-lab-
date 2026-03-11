# Lab 06 — Multi Department Network
Date: March 11, 2026
Topic: Multi Department Network Routing
Stage: Stage 2 — Intermediate
Status: ✅ Completed

---

## What I Built
A company network with 4 departments.
Each department has its own switch and 2 PCs.
All departments connected through one router.
All departments can communicate with each other.

---

## Network Layout
Router (2911)
├── Gig0/0  → Switch1 → PC0, PC1  (IT Department)
├── Gig0/1  → Switch2 → PC2, PC3  (Office Staff)
├── Gig0/2  → Switch3 → PC4, PC5  (Management)
└── Fa0/3/0 → Switch4 → PC6, PC7  (Student)

---

## Step 1 — Added Devices
Opened Packet Tracer and added:
1 x Router (2911)
4 x Switches (Switch-PT)
8 x PCs

Each switch represents one department.
Each department has 2 computers.

---

## Step 2 — Connected Cables

Cable type used for everything:
Copper Straight-Through

Why Copper Straight-Through?
We use Copper Straight-Through when connecting
different types of devices together.
PC to Switch = different devices = Straight-Through
Switch to Router = different devices = Straight-Through

How to add cable in Packet Tracer:
1. Click lightning bolt icon at bottom
2. Choose Copper Straight-Through
3. Click first device and choose port
4. Click second device and choose port
5. Green dots = connected correctly ✅
6. Red dots = problem with connection ❌

Connections we made:
Router Gig0/0 → Switch1 Fa0/1
Router Gig0/1 → Switch2 Fa0/1
Router Gig0/2 → Switch3 Fa0/1
Switch1 Fa0/2 → PC0 Fa0
Switch1 Fa0/3 → PC1 Fa0
Switch2 Fa0/2 → PC2 Fa0
Switch2 Fa0/3 → PC3 Fa0
Switch3 Fa0/2 → PC4 Fa0
Switch3 Fa0/3 → PC5 Fa0

For 4th department we had a problem!
Router had no free Gig ports left.
So we added a module (explained below).

After adding module:
Router Fa0/3/0 → Switch4 Fa0/1
Switch4 Fa0/2  → PC6 Fa0
Switch4 Fa0/3  → PC7 Fa0

---

## Step 3 — Problem With 4th Switch

The Cisco 2911 router only has 3 Gig ports:
Gig0/0 = used by Switch1
Gig0/1 = used by Switch2
Gig0/2 = used by Switch3
Gig0/3 = does not exist!

When we tried to connect Switch4 to router
there was no free port available!

This is very common in real life.
Company grows and needs more ports.
Solution = add a hardware module!

---

## Step 4 — Added HWIC Module to Router

HWIC stands for High Speed WAN Interface Card.
It is a small card that slides into router
and gives extra ports.

How we added it in Packet Tracer:
1. Click on Router
2. Click Physical tab
3. Click power button to turn router OFF
4. Find HWIC-4ESW module on left side
5. Drag it into empty slot on router
6. Click power button to turn router ON
7. Router now has 4 extra ports!

New ports after adding module:
Fa0/3/0 = used for Switch4
Fa0/3/1 = available
Fa0/3/2 = available
Fa0/3/3 = available

In real life this means physically opening
the router and inserting the card into
the empty expansion slot!

---

## Step 5 — Problem With Module Ports

When we tried to give IP address to Fa0/3/0
the router gave an error!

Command we tried:
interface fa0/3/0
ip address 192.168.40.1 255.255.255.0

Error we got:
% Invalid input detected at '^' marker

Why did this happen?
Because Fa0/3/0 is a Layer 2 switch port
not a Layer 3 router port!

Difference between Layer 2 and Layer 3:
Layer 2 = switch port
          works with MAC addresses
          moves data within same network
          CANNOT have IP address ❌

Layer 3 = router port
          works with IP addresses
          routes data between networks
          CAN have IP address ✅

Gig0/0, Gig0/1, Gig0/2 = Layer 3 ports ✅
Fa0/3/0, Fa0/3/1        = Layer 2 ports ❌

---

## Step 6 — Fixed Using VLAN Interface

To solve this we used a VLAN interface.
Also called SVI (Switched Virtual Interface).
This is a virtual interface in software
not a physical port.

Think of it like a virtual door that
has its own address and can communicate
with the outside world on behalf of
all devices connected to Switch4!

Command we used:
interface vlan 1
ip address 192.168.40.1 255.255.255.0
no shutdown

This created a virtual interface called Vlan1
and gave it IP address 192.168.40.1.
This IP became the gateway for Switch4!

This is very common in real networks.
Many companies use VLAN interfaces
as gateways instead of physical ports.

---

## Step 7 — Configured Router Interfaces

en
conf t
interface gig0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
interface gig0/1
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
interface gig0/2
ip address 192.168.30.1 255.255.255.0
no shutdown
exit
interface vlan 1
ip address 192.168.40.1 255.255.255.0
no shutdown
exit

Why no shutdown?
Cisco router ports are OFF by default!
We must type no shutdown to turn them on.
Without this nothing works even if
cables are connected correctly!

---

## Step 8 — IP Plan

Department 1 - IT
Router Gig0/0 = 192.168.10.1
PC0           = 192.168.10.10
PC1           = 192.168.10.11
Gateway       = 192.168.10.1

Department 2 - Office
Router Gig0/1 = 192.168.20.1
PC2           = 192.168.20.10
PC3           = 192.168.20.11
Gateway       = 192.168.20.1

Department 3 - Management
Router Gig0/2 = 192.168.30.1
PC4           = 192.168.30.10
PC5           = 192.168.30.11
Gateway       = 192.168.30.1

Department 4 - Student
Router Vlan1  = 192.168.40.1
PC6           = 192.168.40.10
PC7           = 192.168.40.11
Gateway       = 192.168.40.1

Why different subnets for each department?
Each department is a separate network.
192.168.10.x = IT network
192.168.20.x = Office network
192.168.30.x = Management network
192.168.40.x = Student network

If two departments had same subnet
they would be on same network and
would not need router to communicate!

---

## Step 9 — Configured PCs

Each PC needs 3 things:
1. IP Address  = unique address of that PC
2. Subnet Mask = size of the network
3. Gateway     = router IP for that department

How to configure PC in Packet Tracer:
1. Click on PC
2. Click Desktop tab
3. Click IP Configuration
4. Type IP Address
5. Type Subnet Mask
6. Type Default Gateway
7. Close window

---

## Verification Commands

Check router interfaces:
show ip interface brief

Should show:
Gig0/0  192.168.10.1  up  up ✅
Gig0/1  192.168.20.1  up  up ✅
Gig0/2  192.168.30.1  up  up ✅
Vlan1   192.168.40.1  up  up ✅

Check routing table:
show ip route

Should show:
C 192.168.10.0 via Gig0/0 ✅
C 192.168.20.0 via Gig0/1 ✅
C 192.168.30.0 via Gig0/2 ✅
C 192.168.40.0 via Vlan1  ✅

---

## Test Results

PC0 ping PC2 (IT to Office)
ping 192.168.20.10 = Success ✅

PC0 ping PC4 (IT to Management)
ping 192.168.30.10 = Success ✅

PC0 ping PC6 (IT to Student)
ping 192.168.40.10 = Success ✅

All 4 departments communicating
through router successfully! ✅

---

## Problems Faced and Solutions

Problem 1:
2911 router only has 3 Gig ports
Could not connect 4th switch!
Solution:
Added HWIC-4ESW module to router
Got 4 extra ports Fa0/3/0 to Fa0/3/3

Problem 2:
Fa0/3/0 is Layer 2 switch port
Could not assign IP address directly!
Error: Invalid input detected
Solution:
Used VLAN1 interface as virtual gateway
Assigned 192.168.40.1 to Vlan1

---

## What I Learned

2911 router      = only 3 Gig ports by default
HWIC-4ESW        = hardware module adds ports
Layer 2 port     = switch port cannot have IP
Layer 3 port     = router port can have IP
VLAN interface   = virtual interface can have IP
SVI              = another name for VLAN interface
no shutdown      = always turn on router ports
Default gateway  = router IP for that department
Ping             = tool to test connectivity
Green dots       = cable connected correctly
Red dots         = connection problem

---

## Simple Summary

Not enough ports    = add HWIC module
Module ports = L2   = cannot take IP
Solution            = use VLAN interface
VLAN interface      = acts as virtual gateway
Every PC needs      = IP + Mask + Gateway
Test with ping      = checks connectivity
All departments     = can talk through router
