# 🌐 Day 05 — Ethernet LAN Switching

## 📌 Overview

Ethernet LAN Switching is a fundamental concept in computer networking.

A network switch connects devices within a **Local Area Network (LAN)** and uses **MAC addresses** to make forwarding decisions.

In this topic, I learned how Ethernet frames are created, how switches learn MAC addresses, and how switches forward frames.

I also practiced these concepts using **Cisco Packet Tracer**.

---

# 🎯 Learning Objectives

By the end of this topic, I learned:

- What Ethernet is
- What LAN switching means
- What a MAC address is
- What an Ethernet frame is
- How a switch learns MAC addresses
- How a MAC address table works
- How switches forward frames
- Known unicast
- Unknown unicast
- Broadcast traffic
- Basic switch verification commands
- How to observe switching using Cisco Packet Tracer

---

# 🔹 What is Ethernet?

**Ethernet** is a widely used networking technology for wired Local Area Networks (LANs).

It defines how devices communicate over a wired network.

Ethernet uses:

- MAC addresses
- Ethernet frames
- Switches
- Network interfaces
- Ethernet cables

### Example

    PC1 ─────┐
             │
    PC2 ─────┼──── Switch
             │
    PC3 ─────┘

## 🆔 MAC Address

MAC stands for **Media Access Control**.
A MAC address is a link-layer address associated with a network interface.

**Example:**
`00:1A:2B:3C:4D:5E`

MAC addresses are used for communication within the local network.

### 🔹 MAC Address Structure
A MAC address is typically represented as 48 bits.
It is commonly written as six groups of hexadecimal values.

`00 : 1A : 2B : 3C : 4D : 5E`

---

## 📦 Ethernet Frame

Ethernet transmits data using frames.
A simplified Ethernet frame contains:

    ┌──────────────┬──────────────┬────────────┐
    │ Destination  │ Source MAC   │   Data     │
    │   MAC        │    Address   │            │
    └──────────────┴──────────────┴────────────┘

It also includes fields such as:
- Preamble
- Destination MAC
- Source MAC
- EtherType/Length
- Data
- FCS

---

## 🔄 How a Switch Learns MAC Addresses

A switch builds a MAC address table by examining the source MAC address of incoming Ethernet frames.

Example:
    PC1
    MAC: AAAA.AAAA.AAAA
         |
         |
       Port Fa0/1
         |
       Switch

When the switch receives a frame from PC1, it learns:
`AAAA.AAAA.AAAA → Fa0/1`

The switch stores this information in its MAC address table.

---

## 📋 MAC Address Table

A MAC address table maps MAC addresses to switch ports.

| MAC Address | Port |
| :--- | :--- |
| AAAA.AAAA.AAAA | Fa0/1 |
| BBBB.BBBB.BBBB | Fa0/2 |
| CCCC.CCCC.CCCC | Fa0/3 |

This allows the switch to make forwarding decisions.

---

## 🚦 Frame Forwarding

When a switch receives an Ethernet frame, it checks the destination MAC address. There are different possibilities.

### 🔹 1. Known Unicast
If the destination MAC address exists in the MAC address table, the switch forwards the frame only through the associated port.

    PC1 ─── Switch ─── PC2
                  |
                  └── PC3

If the switch knows that PC2's MAC address is on a particular port:
`Destination MAC` -> `MAC Table Lookup` -> `Correct Port` -> `PC2`

*(The frame is not sent to all ports.)*

### 🔹 2. Unknown Unicast
If the destination MAC address is not present in the MAC address table, the switch generally floods the frame out other appropriate ports in the same VLAN, except the port on which the frame was received.

                 ┌── PC2
                 │
    PC1 ─ Switch ├── PC3
                 │
                 └── PC4

This helps the switch discover where the destination device is located.

### 🔹 3. Broadcast
A broadcast frame is intended for all devices within the local broadcast domain.

    PC1 → Switch
           ↓
     ┌─────┼─────┐
     ↓     ↓     ↓
    PC2   PC3   PC4

The switch forwards the broadcast out other ports in the same VLAN.
A common example is an ARP request.

---

## 🔄 MAC Learning Process

The basic process is:
1. Frame arrives at switch
2. Switch reads source MAC
3. Switch records source MAC + port
4. Switch checks destination MAC
5. Switch decides where to forward

### 🧠 Important Rule
A switch primarily uses:
- **Source MAC** → Learn
- **Destination MAC** → Forward

*(This is one of the most important concepts in Ethernet switching.)*

---

## 🧪 Hands-On Lab — Cisco Packet Tracer

**Lab Objective:** Create a LAN and observe how a switch learns MAC addresses and forwards Ethernet frames.

### 🖥️ Topology
    PC1 ─────┐
             │
    PC2 ─────┼──── SW1
             │
    PC3 ─────┘

### Step 1 — Add Devices
In Cisco Packet Tracer:
- Add 3 PCs
- Add 1 Cisco switch
- Connect each PC to the switch using Ethernet cables

Example:
- PC1 → SW1 Fa0/1
- PC2 → SW1 Fa0/2
- PC3 → SW1 Fa0/3

### Step 2 — Configure IP Addresses
- **PC1:** IP Address: `192.168.1.10`, Subnet Mask: `255.255.255.0`
- **PC2:** IP Address: `192.168.1.20`, Subnet Mask: `255.255.255.0`
- **PC3:** IP Address: `192.168.1.30`, Subnet Mask: `255.255.255.0`

*(All three PCs are in the same subnet.)*

### Step 3 — Test Connectivity
From PC1:
`ping 192.168.1.20`
Then:
`ping 192.168.1.30`
*(Successful replies show that the devices can communicate.)*

### 🔍 Step 4 — Check the MAC Address Table
On the switch CLI:
`SW1# show mac address-table`

You may see output similar to:
    Vlan    Mac Address       Type       Ports
    ----    -----------       --------   -----
    1       AAAA.AAAA.AAAA    DYNAMIC    Fa0/1
    1       BBBB.BBBB.BBBB    DYNAMIC    Fa0/2
    1       CCCC.CCCC.CCCC    DYNAMIC    Fa0/3

### 🔎 Step 5 — Check Interfaces
`SW1# show interfaces`
*(This provides detailed information about switch interfaces.)*

### 🔎 Step 6 — Check Interface Status
`SW1# show interfaces status`
*(This gives a quick overview of switch port status.)*

### 🧪 Simulation Mode
Cisco Packet Tracer provides Simulation Mode, which can help visualize packet movement.

**Steps:**
1. Switch from Realtime to Simulation.
2. Generate traffic between PCs.
3. Observe the packets.
4. Follow the packet as it moves through the switch.
5. Inspect Ethernet information.
6. Observe source and destination MAC addresses.

### 🔄 Example of Switching
Suppose: `PC1 → PC2`
PC1 sends an Ethernet frame containing:
- Source MAC: PC1 MAC
- Destination MAC: PC2 MAC

The switch checks its MAC address table. If PC2's MAC is known, the frame is forwarded only to PC2's port.

---

## 📊 Ethernet Traffic Types

| Traffic Type | Purpose |
| :--- | :--- |
| **Unicast** | One sender → One destination |
| **Broadcast** | One sender → All devices in broadcast domain |
| **Multicast** | One sender → Selected group |

---

## 🔐 Ethernet Switching & Cybersecurity

Understanding Ethernet switching is important for cybersecurity. Security professionals need to understand how attackers and defenders interact with local network traffic.

Important concepts include:
- MAC addresses
- ARP
- VLANs
- Broadcast domains
- Network segmentation
- MAC address tables
- Traffic monitoring

Switching knowledge provides the foundation for understanding attacks such as:
- MAC flooding
- ARP spoofing
- VLAN-related attacks
*(These topics should only be practiced in authorized lab environments.)*

---

## 🛡️ Switch Security Concepts

Later in the networking journey, I will explore security features such as:
- Port Security
- VLAN segmentation
- DHCP Snooping
- Dynamic ARP Inspection
- Spanning Tree security
- Access Control Lists
*(These technologies help protect LAN environments.)*

---

## 🔧 Useful Cisco Commands

| Command | Purpose |
| :--- | :--- |
| `show mac address-table` | Displays the MAC address table. |
| `show interfaces` | Displays detailed interface information. |
| `show interfaces status` | Displays interface status. |
| `show running-config` | Displays the current running configuration. |
| `show vlan brief` | Displays VLAN information. |

---

## 🧠 Key Takeaways

- Ethernet is widely used for wired LAN communication.
- Switches forward Ethernet frames within a LAN.
- Switches use MAC addresses for forwarding decisions.
- A switch learns source MAC addresses.
- The MAC address table maps MAC addresses to switch ports.
- Known unicast traffic can be forwarded to a specific port.
- Unknown unicast traffic is generally flooded within the VLAN.
- Broadcast traffic is forwarded throughout the broadcast domain.
- Cisco Packet Tracer can be used to visualize switching behavior.
- `show mac address-table` is an important switch verification command.

---

## 🧪 Practical Skills Practiced

Using Cisco Packet Tracer, I practiced:
- Creating a LAN topology
- Connecting PCs to a switch
- Configuring IPv4 addresses
- Testing connectivity using ping
- Observing Ethernet communication
- Understanding MAC addresses
- Checking the MAC address table
- Understanding frame forwarding
- Using Simulation Mode
- Checking switch interfaces

### 📚 Commands Learned
`show mac address-table`, `show interfaces`, `show interfaces status`, `show running-config`, `show vlan brief`, `ping`

---

## 🚀 What I Learned

Day 05 helped me understand what happens inside a LAN when devices communicate. I learned that a switch doesn't simply send every frame everywhere. It learns MAC addresses and uses its MAC address table to make forwarding decisions.

This is an important foundation for learning:
- VLANs
- Routing
- Network Security
- Network Troubleshooting
- Enterprise Networking

### 🔗 Connection to Cybersecurity
Networking and cybersecurity are closely connected. Understanding how switches forward frames helps me understand how attacks and defenses work at the LAN level. My next goal is to continue building practical networking knowledge and gradually connect it with cybersecurity.

**✅ Progress: Day 05 — Ethernet LAN Switching — COMPLETED 🚀**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
