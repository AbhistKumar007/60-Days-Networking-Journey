# 🌐 Day 03 - TCP/IP Model

## 📌 Overview

The TCP/IP model is a networking framework used to understand how devices communicate across networks.

It consists of four layers:

1. Application
2. Transport
3. Internet
4. Network Access

---

# 🔹 1. Application Layer

The Application Layer is the top layer of the TCP/IP Model.
It provides network services to applications used by users.

### Common Protocols

- HTTP (Web communication)
- HTTPS (Secure web communication)
- DNS (Converts domain names to IP addresses)
- FTP (File transfer)
- SMTP (Sending emails)
- SSH (Secure remote access)
- DHCP (Automatically assigns IP configuration)

### Example

When I open `https://example.com`, the browser uses application-layer protocols to communicate with the web server.

---

# 🔹 2. Transport Layer

The Transport Layer provides communication between applications running on different devices.

The two major transport protocols are:

### TCP (Transmission Control Protocol)

TCP is:
- Connection-oriented
- Reliable
- Ordered
- Error checked
- Able to retransmit lost data

TCP is commonly used when reliable delivery is important.
Examples: HTTPS, SSH, FTP, SMTP.

### UDP (User Datagram Protocol)

UDP is:
- Connectionless
- Faster
- Lightweight
- Lower overhead
- Does not guarantee delivery

UDP is useful when speed is more important than guaranteed delivery.
Examples: DNS queries, Online gaming, VoIP, Streaming.

### ⚔️ TCP vs UDP

| Feature | TCP | UDP |
| :--- | :--- | :--- |
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable | No delivery guarantee |
| Ordering | Ordered | No ordering guarantee |
| Speed | Generally slower | Generally faster |
| Overhead | Higher | Lower |
| Retransmission | Yes | No |

### 🤝 TCP Three-Way Handshake

Before transmitting TCP data, a connection is established using a three-way handshake.

    Client                         Server
      |                              |
      | -------- SYN --------------> |
      |                              |
      | <------- SYN-ACK ------------|
      |                              |
      | -------- ACK --------------> |
      |                              |
      |       Connection Established |

---

# 🔹 3. Internet Layer

The Internet Layer is responsible for logical addressing and routing packets between networks.

### Important Protocols

- IPv4
- IPv6
- ICMP

### Main Responsibilities

- Logical addressing
- Packet delivery
- Routing
- Inter-network communication

### Examples

- **IPv4:** Uses 32-bit addresses divided into four octets (e.g., `192.168.1.10`).
- **IPv6:** Provides a much larger address space (e.g., `2001:db8::1`).
- **ICMP:** Used for diagnostics (e.g., `ping 192.168.1.1` uses ICMP Echo Requests).

---

# 🔹 4. Network Access Layer

The Network Access Layer is responsible for communication over the local network and the physical transmission of data.

It includes technologies such as:
- Ethernet
- Wi-Fi
- ARP
- MAC addressing
- Network interfaces

### 🆔 MAC Address

A MAC address is a hardware/link-layer address associated with a network interface.
Example: `00:1A:2B:3C:4D:5E`
Switches use MAC addresses to make forwarding decisions for Ethernet frames.

---

# 🔄 Encapsulation & Decapsulation

**Encapsulation** happens when data moves down through the TCP/IP layers before transmission:
`Application Data` -> `TCP Segment` -> `IP Packet` -> `Ethernet Frame` -> `Bits`

**Decapsulation** happens when the receiving device processes the data as it moves up through the layers, removing headers.

### 📦 PDU Names

- Application = Data
- Transport = Segment / Datagram
- Internet = Packet
- Network Access = Frame

---

# 🌍 Real-World Example — Opening a Website

Suppose I open `https://google.com`:

1. **Application Layer:** The browser creates an HTTPS request.
2. **Transport Layer:** TCP establishes a connection and transports the application data.
3. **Internet Layer:** IP adds source and destination IP addresses.
4. **Network Access Layer:** The data is placed into an Ethernet or Wi-Fi frame.

---

# 🔁 OSI Model vs TCP/IP Model

The OSI Model has 7 layers, while the TCP/IP Model commonly has 4 layers.

| OSI Model | TCP/IP Model |
| :--- | :--- |
| Application, Presentation, Session | Application |
| Transport | Transport |
| Network | Internet |
| Data Link, Physical | Network Access |

---

# 🧪 Hands-On Lab — Cisco Packet Tracer

**Lab Objective:** Create a simple network and understand how communication moves through the TCP/IP model.

### Topology

`PC1 ───── Switch ───── PC2`

### Steps

1. **Add Devices:** Add 2 PCs and 1 Switch. Connect the PCs to the switch using Ethernet cables.
2. **Configure IP Addresses:**
   - PC1: `192.168.1.10` (Subnet: 255.255.255.0)
   - PC2: `192.168.1.20` (Subnet: 255.255.255.0)
3. **Test Connectivity:** From PC1 Command Prompt, run:
   `ping 192.168.1.20`

### 🔧 Useful Cisco Commands

- `show ip interface brief` (Displays interfaces, IPs, and statuses)
- `show running-config` (Displays current active configuration)
- `show interfaces` (Provides detailed interface info)
- `ping` (Tests connectivity)

---

# 🔍 Troubleshooting Using TCP/IP Layers

When communication fails, troubleshoot layer by layer:

- **Application:** Check DNS, HTTP/HTTPS, application config.
- **Transport:** Check TCP/UDP, ports, firewall rules.
- **Internet:** Check IP address, subnet mask, default gateway, routing.
- **Network Access:** Check Ethernet cable, NIC, switch port, MAC address, Wi-Fi.

---

# 🔐 TCP/IP Model & Cybersecurity

Understanding TCP/IP is extremely important in cybersecurity. Security professionals need to understand IPs, ports, protocols, and network traffic.

### Tools Used
- Wireshark
- Nmap
- Scapy
- tcpdump

### Example: Network Attack Perspective

`IP Address` -> `Open Ports` -> `Running Services` -> `Potential Vulnerabilities`

Understanding TCP/IP helps a cybersecurity professional understand what is happening at each stage.

---

# 🧠 Key Takeaways

- TCP/IP is the foundation of modern network communication.
- TCP/IP commonly uses four layers.
- TCP provides reliable communication; UDP is lightweight and connectionless.
- IP provides logical addressing and routing.
- Ethernet and Wi-Fi provide local network communication.
- Encapsulation (sending) and Decapsulation (receiving).
- TCP uses a three-way handshake.

### 🚀 Conclusion

The TCP/IP Model helped me understand how data moves from one device to another across a network. Instead of only memorizing the layers, I connected the concepts with a practical Cisco Packet Tracer lab.

> LEARN → PRACTICE → DOCUMENT → IMPROVE
