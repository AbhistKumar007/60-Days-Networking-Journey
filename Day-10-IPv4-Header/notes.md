# Day 10 — IPv4 Header 📦🌐

## 📌 Topic

**IPv4 Header**

Today I learned about the **IPv4 Header** and the important information carried inside an IPv4 packet.

---

## 🎯 Objectives

- Understand the structure of an IPv4 header
- Learn the purpose of important header fields
- Understand how routers use IPv4 header information
- Identify source and destination IP addresses
- Understand TTL and Protocol fields

---

## 📦 IPv4 Header Structure

An IPv4 header contains information required to deliver an IP packet from the source to the destination.

### Main Fields

| Field            | Purpose                                        |
|-------------------|-------------------------------------------------|
| Version           | Identifies the IP version                       |
| IHL               | Specifies the header length                     |
| DSCP/ECN          | Traffic handling and congestion information     |
| Total Length      | Size of the complete IP packet                  |
| Identification    | Used for fragmentation                          |
| Flags             | Controls fragmentation                          |
| Fragment Offset   | Identifies fragment position                    |
| TTL               | Prevents packets from looping forever           |
| Protocol          | Identifies the upper-layer protocol             |
| Header Checksum   | Detects errors in the IPv4 header               |
| Source IP         | IP address of the sender                        |
| Destination IP    | IP address of the receiver                      |
| Options           | Optional additional information                 |

---

## 🔍 Important Fields

### 1. Version

Indicates the IP version.

For IPv4:

```
Version = 4
```

### 2. IHL — Internet Header Length

Specifies the length of the IPv4 header.

The minimum IPv4 header size is:

```
20 bytes
```

### 3. Total Length

Specifies the total size of the IP packet:

```
IPv4 Header + Data
```

Maximum IPv4 packet size:

```
65,535 bytes
```

### 4. TTL — Time To Live

TTL prevents packets from continuously travelling around a network.

Each router normally decreases the TTL value by 1.

Example:

```
TTL 64
 ↓
Router 1 → 63
 ↓
Router 2 → 62
 ↓
Router 3 → 61
```

When TTL reaches zero, the packet is discarded.

### 5. Protocol

Identifies the protocol carried inside the IP packet.

Examples:

```
TCP   → 6
UDP   → 17
ICMP  → 1
```

### 6. Source IP Address

Identifies the device that sent the packet.

Example:

```
Source IP: 192.168.1.10
```

### 7. Destination IP Address

Identifies the intended destination.

Example:

```
Destination IP: 192.168.1.20
```

### 8. Header Checksum

Used to detect errors in the IPv4 header.

---

## 🔄 How an IPv4 Packet Travels

```
Source
  │
  │ IPv4 Packet
  ▼
Router
  │
  │ Reads Destination IP
  ▼
Next Network
  │
  ▼
Destination
```

Routers examine the destination IP address and use their routing table to determine where to forward the packet.

---

## 💻 Practical — Cisco Packet Tracer

I used Cisco Packet Tracer to understand how IPv4 packets travel between devices.

**Basic Test**

```
ping 192.168.1.20
```

I also explored packet information using Simulation Mode to observe how packets move through the network.

---

## 🔐 Cybersecurity Connection

Understanding IPv4 headers is useful for:

- Packet analysis
- Network monitoring
- Firewall configuration
- IDS/IPS
- Traffic analysis
- Network troubleshooting

Tools such as Wireshark can be used to inspect IPv4 headers in real network traffic.

---

## 🧠 Key Takeaways

```
IPv4 Header
    ↓
Source IP
    ↓
Destination IP
    ↓
TTL
    ↓
Protocol
    ↓
Routing & Delivery
```

The IPv4 header provides the information needed for identifying, routing, and delivering IP packets across a network.

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco Simulation Mode
- Ping

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 10/60 — Completed ✅**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
