
# Spanning Tree Protocol (Part 2) | Day 21
## 60 Days Networking Journey

## 📌 Overview

In Day 20, I learned the fundamentals of Spanning Tree Protocol (STP).

Today, I am focusing on practical STP configuration and verification using Cisco Packet Tracer.

### 🎯 Learning Objectives

- Create a redundant Layer 2 topology.
- Observe STP Root Bridge election.
- Identify Root Ports and Designated Ports.
- Understand alternate and blocking paths.
- Configure the Root Bridge manually.
- Observe STP behavior during link failure.
- Verify STP using Cisco IOS commands.

---

# 1. Practical Lab — STP Topology

## 🛠️ Required Devices

- 3 × Cisco 2960 Switches
- 3 × PCs (optional)
- Copper crossover or automatic cable selection

## Network Topology

Connect the switches in a triangle:

```text
              SW1
             /   \
            /     \
          SW2-----SW3
```

Connections:

| Switch | Interface | Connected Switch | Interface |
|---|---|---|---|
| SW1 | Fa0/1 | SW2 | Fa0/1 |
| SW1 | Fa0/2 | SW3 | Fa0/1 |
| SW2 | Fa0/2 | SW3 | Fa0/2 |

This topology creates redundant Layer 2 paths.

**Why this topology?**

Without STP, redundant connections between switches can create Layer 2 loops.

STP identifies the redundant path and prevents it from forwarding normal traffic.

---

# 2. Check STP Status

Open the CLI of each switch.

Run:

```bash
enable
show spanning-tree
```

This command displays:

- Root Bridge ID
- Local Bridge ID
- Root Path Cost
- Root Port
- Designated Ports
- Port States

To check a specific VLAN:

```bash
show spanning-tree vlan 1
```

### Important Output Fields

| Field | Meaning |
|---|---|
| Root ID | Information about the Root Bridge |
| Bridge ID | Information about the local switch |
| Root Cost | Total path cost toward the Root Bridge |
| Root Port | Best path toward the Root Bridge |
| Desg | Designated Port |
| Root | Root Port |
| Altn | Alternate Port |
| FWD | Forwarding State |
| BLK | Blocking State |

**Note:** Exact output labels depend on the STP mode and switch model.

---

# 3. Root Bridge Election

STP elects one switch as the Root Bridge.

The switch with the lowest Bridge ID wins.

Bridge ID consists of:

- Bridge Priority
- MAC Address

If all switches have the same priority, the switch with the lowest MAC address becomes the Root Bridge.

### Check the Root Bridge

```bash
show spanning-tree root
```

Or:

```bash
show spanning-tree vlan 1
```

Look for the Root ID section.

If the Root ID and local Bridge ID match, the local switch is the Root Bridge.

---

# 4. Configure a Root Bridge Manually

For this lab, configure SW1 as the Root Bridge.

### SW1 Configuration

```bash
enable
configure terminal

spanning-tree vlan 1 priority 4096

end
```

### Verify

```bash
show spanning-tree vlan 1
```

Because SW1 has a lower priority than the default priority of the other switches, it should become the Root Bridge, assuming no other switch has a lower Bridge ID.

### Alternative Cisco Command

```bash
configure terminal
spanning-tree vlan 1 root primary
end
```

This command asks the switch to become the primary Root Bridge by adjusting its priority as needed.

---

# 5. Understand STP Port Roles

After Root Bridge election, STP determines port roles.

### Root Port (RP)

- Best path from a non-root switch toward the Root Bridge.
- Every non-root switch has one Root Port per spanning-tree instance.

### Designated Port (DP)

- Forwards traffic for a network segment.
- The Root Bridge's active ports are Designated Ports.

### Alternate Port

- Provides an alternative path toward the Root Bridge.
- Remains in a non-forwarding state in RSTP.

### Example

```text
              SW1
          ROOT BRIDGE
             /   \
            /     \
          SW2-----SW3
```

The exact Root, Designated, and Alternate Port assignments depend on Bridge IDs, path costs, and port IDs.

---

# 6. Understand Blocking and Forwarding

STP prevents loops by stopping selected redundant ports from forwarding traffic.

### Forwarding State

- Forwards normal data frames.
- Learns MAC addresses.
- Participates in normal network communication.

### Blocking State (Traditional STP)

- Does not forward normal data frames.
- Does not learn MAC addresses.
- Continues to process relevant STP BPDUs.

### Why Does STP Block a Port?

Suppose two switches have multiple paths to the Root Bridge.

STP selects the best path and prevents redundant paths from forwarding, keeping the Layer 2 topology loop-free.

**Important:** A blocked port is not necessarily a failed port. It may be a healthy redundant link that STP is intentionally preventing from forwarding.

---

# 7. Test Link Failure Recovery

Now test what happens when a switch connection fails.

### Step 1: Check the Current Topology

Run:

```bash
show spanning-tree vlan 1
```

Identify the forwarding and blocked ports.

### Step 2: Shut Down One Active Link

For example, on SW1:

```bash
enable
configure terminal

interface fastEthernet 0/1
shutdown

end
```

### Step 3: Observe STP

Run:

```bash
show spanning-tree vlan 1
```

Check whether STP selects another available path.

### Step 4: Restore the Link

```bash
configure terminal

interface fastEthernet 0/1
no shutdown

end
```

### Expected Result

- STP detects the topology change.
- A redundant path may become active.
- Connectivity can recover through another available path.

**Note:** Traditional 802.1D STP may take longer to converge than Rapid STP (RSTP). Actual recovery time depends on STP mode, topology, and timers.

---

# 8. Configure PortFast and BPDU Guard

PortFast is intended for switch ports connected to end devices, such as PCs.

It allows an edge port to enter forwarding more quickly.

### Enable PortFast

On a PC-connected interface:

```bash
configure terminal

interface fastEthernet 0/10
spanning-tree portfast

end
```

### Enable BPDU Guard

```bash
configure terminal

interface fastEthernet 0/10
spanning-tree bpduguard enable

end
```

### Why Use BPDU Guard?

If a BPDU is received on a BPDU Guard-enabled edge port, the port is placed into an error-disabled state.

This helps protect the network from unauthorized switches being connected to edge ports.

**Security Warning:** Do not enable PortFast on ordinary switch-to-switch links.

---

# 9. Important STP Commands

| Command | Purpose |
|---|---|
| `show spanning-tree` | Displays STP information for all VLANs. |
| `show spanning-tree vlan 1` | Displays STP information for VLAN 1. |
| `show spanning-tree root` | Displays Root Bridge information. |
| `show spanning-tree summary` | Displays STP summary information. |
| `show spanning-tree interface fa0/1` | Displays STP details for an interface. |
| `show interfaces status` | Displays interface status. |
| `show running-config` | Displays the current configuration. |

---

# 10. Troubleshooting

| Problem | Possible Solution |
|---|---|
| Unexpected Root Bridge | Check Bridge Priority and MAC Address. |
| No redundant port blocked | Check whether the topology actually contains a Layer 2 loop. |
| STP not visible | Check VLAN and switch STP configuration. |
| Connectivity lost | Check port states, cables, and VLAN configuration. |
| Link does not recover | Check interface status and STP convergence. |
| Port becomes error-disabled | Check for BPDU Guard violations. |

---

# 11. Cybersecurity Connection 🔐

STP is important for network availability and Layer 2 security.

### Common STP Security Risks

**1. Rogue Root Bridge**

An unauthorized switch may advertise a superior Bridge ID and become the Root Bridge.

**2. Unexpected BPDU Messages**

Unauthorized devices can introduce unexpected STP information into the network.

**3. Layer 2 Network Disruption**

Incorrect STP configuration can affect network availability and connectivity.

### Security Best Practices

- Configure a trusted Root Bridge.
- Use Root Guard where appropriate.
- Enable BPDU Guard on edge ports.
- Use PortFast only on suitable end-device ports.
- Monitor STP topology changes.
- Secure physical access to switches.

---

# 12. Lab Results

| Task | Status |
|---|---|
| Created a three-switch topology | ✅ |
| Established redundant links | ✅ |
| Checked STP status | ✅ |
| Identified Root Bridge | ✅ |
| Configured Root Bridge priority | ✅ |
| Observed STP port roles | ✅ |
| Tested link failure | ✅ |
| Practiced PortFast | ✅ |
| Practiced BPDU Guard | ✅ |

---

# 📚 What I Learned Today

Today I practiced Spanning Tree Protocol using Cisco Packet Tracer.

I learned how to configure a Root Bridge, identify Root and Designated Ports, understand alternate paths, and observe how STP responds when a network link fails.

I also explored PortFast and BPDU Guard to understand how STP contributes to network security.

This practical lab improved my understanding of Layer 2 switching, redundancy, and network availability.

---

## 🚀 60 Days Networking Journey

**DAY 21/60 — SPANNING TREE PROTOCOL (PART 2) COMPLETED ✅**

LEARN → PRACTICE → DOCUMENT → IMPROVE

#Networking #STP #SpanningTreeProtocol #CiscoPacketTracer #CCNA #Switching #NetworkSecurity #CyberSecurity #LearningInPublic
