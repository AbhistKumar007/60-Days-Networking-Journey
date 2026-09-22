# Day 12 — Life of a Packet | Lab 📦🌐

## 📌 Topic

**Life of a Packet**

Today I explored how a packet travels from a source device to a destination through switches and routers using **Cisco Packet Tracer**.

---

## 🎯 Objectives

- Understand how packets travel through a network
- Observe packet forwarding
- Understand MAC and IP addresses
- Understand switches and routers in packet delivery
- Learn encapsulation and decapsulation
- Use Packet Tracer Simulation Mode

---

## 🖥️ Basic Network Topology

```
PC1
 │
 ▼
Switch
 │
 ▼
Router 1
 │
 ▼
Router 2
 │
 ▼
Switch
 │
 ▼
PC2
```

---

## 📦 What Happens to the Data?

When a device sends data, it passes through different layers before being transmitted across the network.

```
Application Data
       ↓
TCP/UDP Segment
       ↓
IP Packet
       ↓
Ethernet Frame
       ↓
Bits
```

This process is called **encapsulation**.

At the destination, the process is reversed and is called **decapsulation**.

---

## 🔄 Packet Journey

### Step 1 — Source PC

PC1 creates data that needs to be sent to PC2.

```
Source IP      → PC1 IP
Destination IP → PC2 IP
```

### Step 2 — Switch

The switch examines the destination MAC address and forwards the Ethernet frame toward the correct interface.

The switch primarily works with MAC addresses at Layer 2.

### Step 3 — Router

The router examines the destination IP address and checks its routing table.

```
Destination IP
      ↓
Routing Table
      ↓
Next Hop / Interface
```

The router then forwards the packet toward the destination network.

### Step 4 — Destination Network

The packet reaches the destination network and is delivered to PC2 through the switch.

---

## 🧪 Practical Lab — Simulation Mode

In Cisco Packet Tracer:

1. Create the network topology.
2. Configure IP addresses.
3. Configure routing if required.
4. Send a ping from PC1 to PC2.
5. Switch to Simulation Mode.
6. Follow the packet as it moves through the network.
7. Inspect the packet information at each device.

---

## 🔍 What I Observed

During the simulation, I observed:

- Source and destination IP addresses
- Source and destination MAC addresses
- Ethernet frames
- IP packets
- Router forwarding
- Switch forwarding
- TTL changes across routers

---

## 🧠 Switch vs Router

| Device  | Main Information | Main Function                        |
|---------|--------------------|----------------------------------------|
| Switch  | MAC Address        | Forwards frames                        |
| Router  | IP Address         | Forwards packets between networks      |

---

## 🔄 Encapsulation & Decapsulation

**At the Source**

```
Data
 ↓
Segment
 ↓
Packet
 ↓
Frame
 ↓
Bits
```

**At the Destination**

```
Bits
 ↓
Frame
 ↓
Packet
 ↓
Segment
 ↓
Data
```

---

## ⏱️ TTL

TTL (Time To Live) helps prevent packets from looping indefinitely.

When a packet passes through a router, its TTL is normally reduced by 1.

Example:

```
TTL 64
  ↓
Router 1 → 63
  ↓
Router 2 → 62
```

---

## 🔐 Cybersecurity Connection

Understanding packet flow is important for:

- Packet analysis
- Network monitoring
- Firewall configuration
- IDS/IPS
- Traffic analysis
- Network troubleshooting
- Detecting unusual network behavior

Tools such as Wireshark can also be used to inspect packets in real networks.

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Simulation Mode
- Ping
- Cisco IOS CLI

---

## 🧠 Key Takeaway

A packet doesn't simply travel directly from one computer to another.

```
Source
  ↓
Switch
  ↓
Router
  ↓
Router
  ↓
Switch
  ↓
Destination
```

Each network device processes the information relevant to its role and helps move the data toward its destination.

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 12/60 — Completed ✅**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
