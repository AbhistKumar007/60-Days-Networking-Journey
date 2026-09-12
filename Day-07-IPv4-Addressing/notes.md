# 🌐 Day 07 — IPv4 Addressing

## 📌 Overview

IPv4 (Internet Protocol version 4) is one of the most widely used addressing systems in computer networks.

An IPv4 address identifies a device on an IP network and helps devices communicate with each other.

In this topic, I learned the basics of IPv4 addressing, address structure, network and host portions, private and public IP addresses, subnet masks, and CIDR notation.

I also practiced IPv4 configuration using **Cisco Packet Tracer**.

---

## 🎯 Learning Objectives

In this topic, I learned:

- What IPv4 is
- IPv4 address structure
- 32-bit IPv4 addressing
- Octets
- Network portion
- Host portion
- Subnet mask
- CIDR notation
- Public IP addresses
- Private IP addresses
- IPv4 private address ranges
- Basic IPv4 configuration
- IPv4 connectivity testing
- Basic IPv4 troubleshooting

---

## 🔹 What is IPv4?

**IPv4 = Internet Protocol Version 4**

IPv4 provides logical addresses to devices connected to a network.

Example:

```
192.168.1.10
```

An IPv4 address helps identify a device and its location within an IP network.

---

## 🔢 IPv4 Address Structure

IPv4 addresses are 32 bits long.

They are divided into four 8-bit sections called octets.

Example:

```
192.168.1.10
```

Structure:

```
192     .168     .1      .10
 ↓       ↓       ↓        ↓
8 bits  8 bits  8 bits   8 bits
```

Therefore:

```
8 + 8 + 8 + 8 = 32 bits
```

---

## 🔢 Binary Representation

Each IPv4 octet can have a value from:

```
0 → 255
```

Example:

```
192.168.1.10
```

In binary:

```
11000000.10101000.00000001.00001010
```

---

## 🧩 Network Portion and Host Portion

An IPv4 address can be divided into:

```
Network Portion + Host Portion
```

The subnet mask determines which part represents the network and which part represents the host.

Example:

```
IP Address:
192.168.1.10

Subnet Mask:
255.255.255.0
```

Here:

```
Network → 192.168.1
Host    → 10
```

---

## 🛡️ Subnet Mask

A subnet mask determines which part of an IPv4 address belongs to the network and which part identifies the host.

Example:

```
255.255.255.0
```

This is commonly represented as:

```
/24
```

Therefore:

```
192.168.1.10/24
```

means:

```
Network Bits → 24
Host Bits    → 8
```

---

## 🔹 CIDR Notation

CIDR stands for:

```
Classless Inter-Domain Routing
```

CIDR notation represents the network prefix using `/`.

Examples:

```
192.168.1.10/24
192.168.1.10/25
192.168.1.10/26
192.168.1.10/27
```

The number after `/` represents the number of network bits.

---

## 📊 Common CIDR Examples

| CIDR | Subnet Mask       | Host Bits |
|------|--------------------|-----------|
| /24  | 255.255.255.0      | 8         |
| /25  | 255.255.255.128    | 7         |
| /26  | 255.255.255.192    | 6         |
| /27  | 255.255.255.224    | 5         |
| /28  | 255.255.255.240    | 4         |
| /29  | 255.255.255.248    | 3         |
| /30  | 255.255.255.252    | 2         |

---

## 🏠 Private IPv4 Addresses

Private IP addresses are used inside local/private networks.

The major private IPv4 ranges are:

| Range                          | CIDR             |
|---------------------------------|-------------------|
| 10.0.0.0 – 10.255.255.255        | 10.0.0.0/8        |
| 172.16.0.0 – 172.31.255.255      | 172.16.0.0/12     |
| 192.168.0.0 – 192.168.255.255    | 192.168.0.0/16    |

**Examples**

```
10.0.0.10
172.16.1.10
192.168.1.10
```

These addresses are commonly used within homes, offices, labs, and private networks.

---

## 🌍 Public IPv4 Address

A public IPv4 address is globally routable on the Internet.

It can be assigned to network devices or services that need Internet connectivity.

Example:

```
Public IP
    ↓
Internet
    ↓
Router
    ↓
Private Network
```

Home networks commonly use NAT to allow private devices to communicate with the Internet through a public address.

---

## 🆚 Private vs Public IP

| Feature                  | Private IP         | Public IP              |
|---------------------------|---------------------|--------------------------|
| Used in                   | Private networks    | Internet                 |
| Globally routable         | No                  | Yes                       |
| Common examples           | 192.168.x.x         | ISP-assigned address     |
| Used inside LAN           | Yes                 | Sometimes                |
| NAT commonly involved     | Yes                 | Often at network edge    |

---

## 📍 Network Address

The network address identifies the network itself.

Example:

```
IP Address:
192.168.1.10/24
```

Network:

```
192.168.1.0
```

So:

```
Network Address = 192.168.1.0
```

---

## 📢 Broadcast Address

The broadcast address is used to communicate with all hosts within an IPv4 subnet.

For:

```
192.168.1.0/24
```

the broadcast address is:

```
192.168.1.255
```

---

## 💻 Usable Host Range

For:

```
192.168.1.0/24
```

the typical usable host range is:

```
192.168.1.1 → 192.168.1.254
```

Therefore:

```
Network Address:
192.168.1.0

Usable Hosts:
192.168.1.1 - 192.168.1.254

Broadcast:
192.168.1.255
```

---

## 🧮 Number of Hosts

For a typical IPv4 subnet:

```
Usable Hosts = 2^h - 2
```

where:

```
h = number of host bits
```

For /24:

```
Host bits = 32 - 24
          = 8
```

Therefore:

```
2^8 - 2
= 256 - 2
= 254 usable hosts
```

---

## 🧪 Hands-On Lab — Cisco Packet Tracer

### 🎯 Objective

Configure IPv4 addresses on PCs and test communication using Cisco Packet Tracer.

### 🖥️ Topology

```
PC1 ─────┐
         │
         │
       Switch
         │
         │
PC2 ─────┘
```

### Step 1 — Add Devices

In Cisco Packet Tracer:

- Add 2 PCs
- Add 1 switch
- Connect both PCs to the switch using Ethernet cables

### Step 2 — Configure PC1

Use:

```
IP Address:
192.168.1.10

Subnet Mask:
255.255.255.0
```

### Step 3 — Configure PC2

Use:

```
IP Address:
192.168.1.20

Subnet Mask:
255.255.255.0
```

Both devices belong to:

```
192.168.1.0/24
```

### Step 4 — Test Connectivity

Open the Command Prompt on PC1.

Run:

```
ping 192.168.1.20
```

If the configuration is correct, PC1 should receive replies from PC2.

---

## 🔍 Understanding the Ping

When PC1 sends:

```
ping 192.168.1.20
```

the network checks whether PC2 is reachable.

A successful test may look like:

```
Reply from 192.168.1.20
Reply from 192.168.1.20
Reply from 192.168.1.20
```

This confirms basic IP connectivity between the two devices.

---

## 🔧 IPv4 Troubleshooting

If communication does not work, check:

**1. IP Address**
Make sure each device has the correct IP.

**2. Subnet Mask**
Make sure devices that should be in the same subnet have compatible subnet masks.

**3. Cable**
Check the physical connection.

**4. Interface Status**
Check whether the network interface is active.

**5. Ping**
Use:

```
ping <destination-IP>
```

---

## 🔐 IPv4 & Cybersecurity

Understanding IPv4 is essential for cybersecurity.

Security professionals work with IP addresses when:

- Monitoring network traffic
- Investigating attacks
- Configuring firewalls
- Performing network scans
- Creating security rules
- Analyzing logs
- Configuring VPNs
- Segmenting networks

Tools such as:

- Wireshark
- Nmap
- Scapy
- tcpdump

work heavily with IP-based networking.

---

## 🛡️ Example — Network Security

Suppose a security administrator wants to monitor:

```
192.168.1.0/24
```

They need to understand:

```
Network Address
      ↓
Host Range
      ↓
Devices
      ↓
Traffic
      ↓
Security Rules
```

This makes IPv4 knowledge an important foundation for network security.

---

## 🧠 Key Takeaways

- IPv4 addresses are 32 bits.
- IPv4 addresses contain four octets.
- Each octet ranges from 0 to 255.
- A subnet mask separates network and host portions.
- CIDR notation uses `/` to represent the network prefix.
- Private IP addresses are commonly used inside local networks.
- Public IP addresses are globally routable.
- The network address identifies the subnet.
- The broadcast address reaches all hosts in the subnet.
- IPv4 knowledge is essential for networking and cybersecurity.

---

## 💻 Practical Skills Practiced

Using Cisco Packet Tracer, I practiced:

- Creating a basic LAN
- Configuring IPv4 addresses
- Configuring subnet masks
- Understanding network addresses
- Testing connectivity
- Using ping
- Basic IPv4 troubleshooting

---

## 📚 Important Examples

**Example 1**
```
IP: 192.168.1.10/24
Network: 192.168.1.0
Broadcast: 192.168.1.255
Usable Hosts: 192.168.1.1 - 192.168.1.254
```

**Example 2**
```
IP: 10.0.0.10/8
Network: 10.0.0.0
```

**Example 3**
```
IP: 172.16.5.20/16
Network: 172.16.0.0
```

---

## 🔄 IPv4 Communication Flow

```
Application
     ↓
Transport Layer
     ↓
IPv4
     ↓
Ethernet / Wi-Fi
     ↓
Network
     ↓
Destination Device
```

---

## 🚀 Why IPv4 Matters

IPv4 is one of the most important foundations of networking.

Understanding IPv4 will help me learn:

- Subnetting
- Routing
- VLANs
- DHCP
- NAT
- ACLs
- Firewalls
- Network Security
- Cloud Networking

---

## 📈 Journey Progress

| Day | Topic                   | Status |
|-----|--------------------------|--------|
| 01  | Networking Devices       | ✅     |
| 02  | Interfaces & Cables      | ✅     |
| 03  | TCP/IP Model             | ✅     |
| 04  | Intro to CLI             | ✅     |
| 05  | Ethernet LAN Switching   | ✅     |
| 06  | Ethernet Switching Lab   | ✅     |
| 07  | IPv4 Addressing          | ✅     |

---

## ✅ Status

**Day 07 — IPv4 Addressing — COMPLETED 🚀**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
