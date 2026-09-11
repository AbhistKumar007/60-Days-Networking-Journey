

# Day 06 — Ethernet Switching Lab

## 🔧 Devices Used

| Device          | Quantity |
|-----------------|----------|
| PC              | 3        |
| Switch          | 1        |
| Ethernet Cable  | 3        |

**Tool:** Cisco Packet Tracer

---

## 🌐 IP Address Configuration

**PC1**
- IP Address: `192.168.1.10`
- Subnet Mask: `255.255.255.0`

**PC2**
- IP Address: `192.168.1.20`
- Subnet Mask: `255.255.255.0`

**PC3**
- IP Address: `192.168.1.30`
- Subnet Mask: `255.255.255.0`

All three devices belong to the same network:

```
192.168.1.0/24
```

---

## 🧪 Lab 1 — Test Connectivity

From PC1:

```
ping 192.168.1.20
```

Then:

```
ping 192.168.1.30
```

If the configuration is correct, PC1 should receive replies.

---

## 🔍 Lab 2 — Check MAC Address Table

On the switch:

```
SW1# show mac address-table
```

The switch learns the MAC address of connected devices and associates each MAC address with a switch port.

Example:

```
MAC Address       Port
-----------------------
AAAA.AAAA.AAAA    Fa0/1
BBBB.BBBB.BBBB    Fa0/2
CCCC.CCCC.CCCC    Fa0/3
```

The actual MAC addresses will depend on the devices in Packet Tracer.

---

## 🔄 How MAC Learning Works

```
Incoming Frame
      ↓
Read Source MAC
      ↓
Learn MAC + Port
      ↓
Check Destination MAC
      ↓
Forward / Flood Frame
```

**Important concept**
- Source MAC → Learn
- Destination MAC → Forward

---

## 🔎 Lab 3 — Check Interface Status

```
SW1# show interfaces status
```

This provides a quick overview of switch ports. You can check whether ports are:

- Connected
- Not connected
- Disabled

---

## 🔎 Lab 4 — Check Interface Information

```
SW1# show interfaces
```

This provides detailed information about the interfaces. It can help identify:

- Interface status
- Traffic
- Errors
- Speed
- Duplex
- Packets

---

## 🔎 Lab 5 — Check VLAN Information

```
SW1# show vlan brief
```

This command displays VLAN information and the ports associated with VLANs. For a basic switch configuration, the PCs may initially be connected to the default VLAN.

---

## 📡 Ethernet Frame Flow

Suppose PC1 communicates with PC2:

```
PC1
 |
 | Ethernet Frame
 ↓
SW1
 |
 | Forward based on MAC
 ↓
PC2
```

The Ethernet frame contains information such as:

- Source MAC
- Destination MAC
- Data
- FCS

### 🔹 Known Unicast

If the switch already knows the destination MAC address:

```
PC1 → Switch → PC2
```

The switch forwards the frame through the correct port.

### 🔹 Unknown Unicast

If the destination MAC address is not in the MAC address table, the switch generally floods the frame within the same VLAN, excluding the incoming port.

```
             PC2
              ↑
              |
PC1 → Switch ─┼→ PC3
              |
              └→ Other Ports
```

Once the switch learns the destination MAC address, future frames can be forwarded more efficiently.

### 🔹 Broadcast

Broadcast traffic is sent to all devices within the same broadcast domain.

Example:

```
PC1 → Switch
       ↓
   ┌───┼───┐
   ↓   ↓   ↓
  PC2 PC3 Other
```

ARP requests are a common example of broadcast traffic in IPv4 networks.

---

## 🧪 Packet Tracer Simulation

I also used Simulation Mode to observe packet movement.

**Steps**
1. Open the Packet Tracer topology.
2. Switch from Realtime to Simulation.
3. Generate traffic between two PCs.
4. Observe the packet.
5. Inspect the Ethernet information.
6. Observe how the switch processes the frame.

This helped me visualize what happens to network traffic instead of only studying it theoretically.

---

## 🔧 Important Cisco Commands

```
show mac address-table
show interfaces
show interfaces status
show vlan brief
show running-config
```

Connectivity testing:

```
ping <destination-IP>
```

---

## 🛠️ Basic Troubleshooting

If two PCs cannot communicate, I can check:

**1. Physical Connection**
```
Cable → Port → Device
```

**2. IP Configuration**
- IP Address
- Subnet Mask

**3. Interface Status**
```
show interfaces status
```

**4. MAC Address Table**
```
show mac address-table
```

**5. Connectivity**
```
ping <destination-IP>
```

---

## 🔐 Cybersecurity Connection

Ethernet switching is an important foundation for network security. Understanding switching helps me understand concepts such as:

- MAC address security
- VLAN segmentation
- Port Security
- ARP security
- Network monitoring
- LAN attacks
- Network access control

Later, I will explore these security concepts in more detail.

---

## 🧠 Key Learnings

- A switch connects devices within a LAN.
- Ethernet communication uses frames.
- MAC addresses are important for LAN communication.
- Switches learn source MAC addresses.
- MAC address tables map MAC addresses to switch ports.
- Known unicast traffic can be forwarded to a specific port.
- Unknown unicast traffic can be flooded within the VLAN.
- Broadcast traffic is forwarded within the broadcast domain.
- Cisco IOS commands help monitor and troubleshoot switches.
- Packet Tracer makes it easier to understand network behavior visually.

---

## 💻 Practical Skills

During this lab, I practiced:

- Cisco Packet Tracer
- LAN topology creation
- IPv4 configuration
- Ping testing
- MAC address learning
- MAC address table verification
- Switch interface verification
- VLAN verification
- Basic troubleshooting
- Ethernet frame analysis

---

## 📚 Commands Practiced

```
show mac address-table
show interfaces
show interfaces status
show vlan brief
show running-config
ping 192.168.1.20
ping 192.168.1.30
```

---

## 🚀 Why This Lab Matters

This practical helped me understand what happens inside a LAN when devices communicate. Instead of only memorizing networking concepts, I practiced them using Cisco Packet Tracer.

This knowledge will be useful for upcoming topics such as:

- IPv4 Addressing
- Subnetting
- VLANs
- Routing
- Network Security
- Network Troubleshooting

---

## 🔗 Networking → Cybersecurity

Strong networking fundamentals are essential for cybersecurity. Before securing a network, I need to understand:

```
How devices communicate
        ↓
How traffic moves
        ↓
How switches forward frames
        ↓
How networks are structured
        ↓
How networks can be secured
```

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

---

## ✅ Status

**Day 06 — Completed 🚀**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
