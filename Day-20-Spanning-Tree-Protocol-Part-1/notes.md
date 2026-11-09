
# Spanning Tree Protocol (STP) — Part 1 | Day 20
## 60 Days Networking Journey

## 📌 Overview

Today I learned about **Spanning Tree Protocol (STP)**, an important Layer 2 switching protocol used to prevent network loops in Ethernet networks.

STP allows switches to have redundant connections while logically blocking selected paths to prevent switching loops.

---

## 🎯 Learning Objectives

- Understand Spanning Tree Protocol.
- Learn why Layer 2 loops occur.
- Understand broadcast storms and MAC address table instability.
- Learn Root Bridge election.
- Understand STP Bridge ID and Path Cost.
- Learn STP port roles and port states.
- Understand how STP maintains a loop-free network.

---

# 1. What is Spanning Tree Protocol (STP)?

STP stands for **Spanning Tree Protocol**.

It is a Layer 2 protocol that prevents switching loops in Ethernet networks by placing selected redundant switch ports into a blocking state.

STP was originally standardized as IEEE 802.1D.

### Why Do We Need STP?

In a switched network, multiple connections between switches provide redundancy.

However, redundant Layer 2 paths can create switching loops.

STP solves this problem by:

- Identifying redundant network paths.
- Electing a Root Bridge.
- Selecting the best paths toward the Root Bridge.
- Blocking selected redundant paths.
- Maintaining a loop-free logical topology.

**Important:** STP does not physically remove redundant connections. It logically blocks selected ports while keeping them available as backup paths.

---

# 2. What is a Layer 2 Network Loop?

A Layer 2 loop occurs when Ethernet frames can travel continuously through multiple interconnected switches.

Unlike IP packets, traditional Ethernet frames do not have a TTL field to limit how many times they can circulate.

### Example

Imagine three switches connected in a triangle:

- Switch 1
- Switch 2
- Switch 3

All three switches are connected to each other.

This creates multiple paths for Ethernet frames.

If STP is not operating, broadcast and unknown unicast frames may circulate repeatedly.

### Problems Caused by Switching Loops

**1. Broadcast Storm**

Broadcast frames continuously circulate and multiply across the network.

**2. MAC Address Table Instability**

Switches may learn the same source MAC address on different ports repeatedly.

This is also called MAC address flapping.

**3. Duplicate Frames**

The destination device may receive multiple copies of the same frame.

**4. Network Performance Issues**

Excessive traffic can consume bandwidth and affect network availability.

---

# 3. How STP Works

STP uses special messages called **Bridge Protocol Data Units (BPDUs)** to exchange information between switches.

Using BPDU information, switches:

1. Elect a Root Bridge.
2. Calculate the best path toward the Root Bridge.
3. Select appropriate port roles.
4. Place redundant ports into a blocking state.
5. Maintain a loop-free Layer 2 topology.

### What is a BPDU?

BPDU stands for **Bridge Protocol Data Unit**.

It is a control message exchanged between switches to share STP information.

BPDUs contain information such as:

- Root Bridge ID.
- Sender Bridge ID.
- Root Path Cost.
- Port information.

---

# 4. Root Bridge Election

The Root Bridge is the central reference point for STP path calculations.

Every STP-enabled switch participates in the Root Bridge election.

### How is the Root Bridge Selected?

The switch with the **lowest Bridge ID (BID)** becomes the Root Bridge.

### What is Bridge ID?

Bridge ID consists of:

- Bridge Priority.
- MAC Address.

The priority is compared first. If priorities are equal, the lowest MAC address wins.

### Default Bridge Priority

The traditional default STP bridge priority is:

`32768`

The extended system ID is included in the operational priority calculation.

### Example

| Switch | Priority | MAC Address |
|---|---:|---|
| SW1 | 32768 | 0001.1111.1111 |
| SW2 | 32768 | 0002.2222.2222 |
| SW3 | 32768 | 0003.3333.3333 |

**Result:** SW1 becomes the Root Bridge because it has the lowest Bridge ID.

---

# 5. STP Port Roles

STP assigns different roles to switch ports to maintain a loop-free topology.

| Port Role | Description |
|---|---|
| Root Port (RP) | The best path from a non-root switch toward the Root Bridge. |
| Designated Port (DP) | The forwarding port for a network segment. |
| Alternate Port | A backup path toward the Root Bridge in RSTP. |
| Backup Port | A backup port for a shared network segment in RSTP. |

### Important Rules

- The Root Bridge has all its active STP ports as Designated Ports.
- Every non-root switch selects one Root Port per spanning-tree instance.
- Each network segment has a Designated Port.
- Redundant paths are prevented from forwarding traffic.

**Note:** Alternate and Backup are commonly used role names in Rapid Spanning Tree Protocol (RSTP). Traditional 802.1D STP describes non-designated ports differently.

---

# 6. STP Port States

Traditional IEEE 802.1D STP uses five port states.

| Port State | Description |
|---|---|
| Disabled | The port is administratively disabled or not participating. |
| Blocking | Does not forward normal data frames or learn MAC addresses. |
| Listening | Processes BPDUs but does not forward data frames or learn MAC addresses. |
| Learning | Learns MAC addresses but does not forward data frames. |
| Forwarding | Forwards data frames and learns MAC addresses. |

### Traditional STP Port State Progression

```text
Blocking
   |
   v
Listening
   |
   v
Learning
   |
   v
Forwarding
```

A port selected to remain blocked stays in the blocking state until the topology changes.

**Important:** Disabled is not one of the normal transition states in the STP progression.

### STP Timers

Traditional STP uses three important timers:

| Timer | Default Value | Purpose |
|---|---:|---|
| Hello Time | 2 seconds | Interval between configuration BPDUs generated by the Root Bridge. |
| Forward Delay | 15 seconds | Time spent in Listening and Learning states. |
| Max Age | 20 seconds | Time a switch retains STP information before considering it outdated. |

---

# 7. STP Path Cost

STP uses path cost to determine the best route toward the Root Bridge.

A lower total path cost is preferred.

Path cost is based primarily on link bandwidth.

### Common IEEE STP Path Costs

| Link Speed | Path Cost |
|---|---:|
| 10 Mbps | 100 |
| 100 Mbps | 19 |
| 1 Gbps | 4 |
| 10 Gbps | 2 |

These are common values using the short path-cost method.

### Example

Suppose a switch has two possible paths to the Root Bridge:

- Path A: Total cost = 19
- Path B: Total cost = 4

STP prefers Path B because it has the lower cost.

If costs tie, STP uses additional tie-breakers based on Bridge ID and Port ID information.

---

# 8. STP Configuration in Cisco IOS

STP is enabled by default on many Cisco Layer 2 switches.

### Check STP Status

```bash
show spanning-tree
```

This command displays:

- Root Bridge information.
- Bridge ID.
- Root path cost.
- Port roles.
- Port states.

### Check STP for a Specific VLAN

```bash
show spanning-tree vlan 1
```

### Check Root Bridge Information

```bash
show spanning-tree root
```

### Change Bridge Priority

Example: Configure Switch 1 with a lower priority.

```bash
configure terminal
spanning-tree vlan 1 priority 4096
end
```

A lower priority makes the switch more likely to become the Root Bridge.

**Note:** The priority must use valid increments of 4096 on common Cisco implementations.

### Verify Configuration

```bash
show spanning-tree vlan 1
```

---

# 9. STP vs RSTP

| Feature | STP | RSTP |
|---|---|---|
| Standard | IEEE 802.1D | IEEE 802.1w |
| Convergence | Generally slower | Generally faster |
| Port Roles | Root, Designated, Non-designated | Root, Designated, Alternate, Backup |
| Port States | Blocking, Listening, Learning, Forwarding, Disabled | Discarding, Learning, Forwarding |
| Purpose | Prevent Layer 2 loops | Prevent loops with faster convergence |

RSTP is designed to restore connectivity faster after topology changes.

---

# 10. Cybersecurity Connection 🔐

STP is important for network availability and Layer 2 security.

### Potential STP Security Risks

**1. Rogue Root Bridge**

An unauthorized switch may advertise a superior Bridge ID and attempt to become the Root Bridge.

**2. BPDU Manipulation**

Unexpected or malicious BPDUs may influence the spanning-tree topology.

### Security Best Practices

- Use a trusted switch as the Root Bridge.
- Configure Root Guard on ports where another switch should not become the Root Bridge.
- Enable BPDU Guard on appropriate edge/access ports.
- Enable PortFast on suitable end-device access ports.
- Monitor unexpected topology changes.
- Restrict physical access to network equipment.

**Important:** PortFast should be used on edge ports connected to end devices, not ordinary switch-to-switch links.

---

# 11. Key Takeaways

- STP prevents Layer 2 switching loops.
- Layer 2 loops can cause broadcast storms and MAC address instability.
- STP uses BPDUs to exchange topology information.
- The lowest Bridge ID wins the Root Bridge election.
- Root Ports provide the best path toward the Root Bridge.
- Designated Ports forward traffic for their network segments.
- STP blocks redundant paths to maintain a loop-free topology.
- RSTP provides faster convergence than traditional STP.

---

# 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- Cisco Layer 2 Switches
- Ethernet Networking

---

# 📚 What I Learned Today

Today I learned the fundamentals of Spanning Tree Protocol and how it prevents Layer 2 switching loops.

I explored Root Bridge election, Bridge ID, BPDU messages, port roles, port states, and path cost.

I also understood why STP is important for network stability, redundancy, and security.

---

## 🚀 60 Days Networking Journey

**DAY 20/60 — SPANNING TREE PROTOCOL (PART 1) COMPLETED ✅**

LEARN → PRACTICE → DOCUMENT → IMPROVE

#Networking #STP #SpanningTreeProtocol #CiscoPacketTracer #CCNA #Switching #NetworkSecurity #CyberSecurity #LearningInPublic
