
# Root Guard (STP Toolkit) | Day 24
## 60 Days Networking Journey

## 📌 Overview

Today I learned about **Root Guard**, an important Cisco Spanning Tree Protocol (STP) security feature.

Root Guard helps protect the Root Bridge position by preventing a switch connected through a protected port from becoming the Root Bridge.

It is useful when we want to maintain control over which switch becomes the Root Bridge in a network.

---

## 🎯 Learning Objectives

- Understand Root Guard.
- Learn why Root Guard is required.
- Understand Root Bridge election.
- Learn how superior BPDUs affect STP.
- Configure Root Guard using Cisco IOS commands.
- Understand the root-inconsistent state.
- Verify Root Guard configuration.
- Practice Root Guard in Cisco Packet Tracer.
- Understand Root Guard security benefits.

---

# 1. What is Root Guard?

Root Guard is a Cisco STP security feature that prevents an unexpected switch from becoming the Root Bridge through a protected interface.

It ensures that the intended Root Bridge remains the Root Bridge.

### Why Do We Need Root Guard?

In a Layer 2 network, STP elects a Root Bridge based on the lowest Bridge ID.

If an unauthorized or incorrectly configured switch sends superior BPDUs, it may influence the Root Bridge election.

This can change the network topology and affect traffic paths.

Root Guard helps prevent this situation.

### Important Points

- Root Guard is configured on a switch interface.
- It protects the intended Root Bridge election.
- It monitors incoming BPDUs.
- It reacts to superior BPDUs.
- It places the affected interface into a root-inconsistent state.

---

# 2. What is a Superior BPDU?

A superior BPDU is an STP BPDU that advertises better Root Bridge information than the switch currently accepts.

For example:

- SW1 is the intended Root Bridge.
- SW2 is connected downstream.
- SW3 is an unauthorized switch.
- SW3 sends superior BPDUs toward SW2.

Without Root Guard, STP may accept the superior information and change the Root Bridge election.

With Root Guard enabled on the appropriate SW2 interface, the port is prevented from accepting that superior Root Bridge information.

---

# 3. How Does Root Guard Work?

Root Guard monitors BPDUs received on a protected interface.

### Working Process

1. The network has an intended Root Bridge.
2. Root Guard is enabled on a designated interface.
3. The interface receives a superior BPDU.
4. Root Guard detects the superior BPDU.
5. The interface enters the root-inconsistent state.
6. The interface stops forwarding traffic.
7. When superior BPDUs stop arriving, the port automatically recovers.

### Important

Root Guard does not shut down the entire switch.

It places the affected interface into a special STP inconsistent state.

---

# 4. Root Guard Configuration

Root Guard is configured on the interface where we do not want another switch to influence the Root Bridge election.

### Step 1: Enter Privileged Mode

```bash
enable
```

### Step 2: Enter Configuration Mode

```bash
configure terminal
```

### Step 3: Select the Interface

```bash
interface fastEthernet 0/24
```

### Step 4: Enable Root Guard

```bash
spanning-tree guard root
```

### Complete Configuration

```bash
enable
configure terminal

interface fastEthernet 0/24
spanning-tree guard root

end
```

### Explanation

| Command | Purpose |
|---|---|
| `enable` | Enters privileged EXEC mode. |
| `configure terminal` | Enters global configuration mode. |
| `interface fastEthernet 0/24` | Selects the interface. |
| `spanning-tree guard root` | Enables Root Guard on the interface. |
| `end` | Exits configuration mode. |

---

# 5. Where Should We Configure Root Guard?

Root Guard should be configured on designated ports where we want to prevent a downstream switch from becoming the Root Bridge.

### Example Topology

```text
             SW1
         ROOT BRIDGE
              |
              |
        SW2 (Protected)
              |
         Root Guard
              |
             SW3
       Unauthorized Switch
```

In this example:

- SW1 is the intended Root Bridge.
- SW2 connects to SW1.
- Root Guard is enabled on SW2's designated port toward SW3.
- SW3 sends superior BPDUs.
- The protected port enters root-inconsistent state.

**Important:** Root Guard should generally be placed on designated ports facing downstream switches, not on the port that should provide the path toward the legitimate Root Bridge.

---

# 6. What is Root-Inconsistent State?

Root-inconsistent is a special STP state caused by Root Guard.

It occurs when a protected port receives superior BPDUs.

### What Happens?

- The port stops forwarding normal traffic.
- The port does not become the Root Port.
- The intended Root Bridge remains protected.
- The port automatically recovers when superior BPDUs stop arriving.

### Verify Root-Inconsistent State

```bash
show spanning-tree inconsistentports
```

You can also check:

```bash
show spanning-tree
```

---

# 7. Cisco Packet Tracer Practical Lab

## Lab Objective

Configure Root Guard and observe how it prevents an unexpected switch from influencing the Root Bridge election.

### Required Devices

- 3 × Cisco 2960 Switches
- Ethernet cables

### Topology

```text
              SW1
          ROOT BRIDGE
               |
               |
              SW2
               |
          ROOT GUARD
               |
              SW3
        Lower Bridge ID
```

### Device Roles

| Switch | Role |
|---|---|
| SW1 | Intended Root Bridge |
| SW2 | Intermediate Switch |
| SW3 | Downstream Switch |

---

## Part A: Configure SW1 as Root Bridge

On SW1:

```bash
enable
configure terminal

spanning-tree vlan 1 priority 4096

end
```

Verify:

```bash
show spanning-tree vlan 1
```

---

## Part B: Configure Root Guard on SW2

On SW2, select the interface connected to SW3.

Example:

```bash
enable
configure terminal

interface fastEthernet 0/2
spanning-tree guard root

end
```

---

## Part C: Configure SW3 with a Lower Priority

On SW3:

```bash
enable
configure terminal

spanning-tree vlan 1 priority 0

end
```

This gives SW3 a lower priority value than SW1.

If SW3 sends superior BPDUs toward SW2, Root Guard should protect the configured interface.

---

## Part D: Verify Root Guard

On SW2:

```bash
show spanning-tree inconsistentports
```

Also run:

```bash
show spanning-tree vlan 1
```

### Expected Result

If the topology and BPDU conditions are correct:

- SW1 remains the Root Bridge.
- SW3 cannot become the Root Bridge through the protected port.
- SW2's protected interface enters root-inconsistent state.

**Note:** Packet Tracer support and exact behavior may vary by switch model. If the command is unsupported, check the device's IOS capabilities.

---

# 8. Recover the Root-Inconsistent Port

Root Guard recovery is automatic.

When superior BPDUs stop arriving on the protected interface, the interface can return to normal STP operation.

### Steps

1. Remove the unexpected switch or correct its STP configuration.
2. Stop the superior BPDU advertisements.
3. Wait for the switch to detect that the superior BPDUs have stopped.
4. Verify the interface state.

### Verification

```bash
show spanning-tree inconsistentports
show spanning-tree vlan 1
```

The interface should no longer appear as root-inconsistent.

---

# 9. Root Guard vs BPDU Guard vs BPDU Filter

| Feature | Root Guard | BPDU Guard | BPDU Filter |
|---|---|---|---|
| Purpose | Protects Root Bridge election | Protects edge ports | Suppresses BPDU exchange |
| Trigger | Superior BPDU | Any received BPDU | Configured BPDU filtering |
| Result | Root-inconsistent state | Error-disabled state | BPDU transmission/reception suppressed |
| Common Use | Designated downstream ports | PortFast edge ports | Carefully controlled edge scenarios |
| Main Benefit | Prevents unexpected Root Bridge changes | Prevents unauthorized switches on edge ports | Controls BPDU exchange |

### Remember

- Root Guard protects Root Bridge election.
- BPDU Guard protects edge ports.
- BPDU Filter suppresses BPDUs and must be used carefully.

---

# 10. Important Verification Commands

| Command | Purpose |
|---|---|
| `show spanning-tree` | Displays STP information. |
| `show spanning-tree vlan 1` | Displays STP information for VLAN 1. |
| `show spanning-tree root` | Displays Root Bridge information. |
| `show spanning-tree inconsistentports` | Displays inconsistent STP ports. |
| `show running-config` | Displays current configuration. |
| `show spanning-tree interface fa0/2 detail` | Displays detailed STP interface information. |
| `show logging` | Displays switch log messages. |

---

# 11. Troubleshooting

| Problem | Possible Solution |
|---|---|
| Root Bridge unexpectedly changes | Check Bridge Priority and Root Guard placement. |
| Port enters root-inconsistent state | Check which connected device is sending superior BPDUs. |
| Interface does not forward traffic | Check STP port role and inconsistent state. |
| Root Guard is not working | Verify the correct interface and VLAN STP configuration. |
| Root Bridge election is incorrect | Check Bridge ID, priority, and topology. |

---

# 12. Cybersecurity Connection 🔐

Root Guard helps protect Layer 2 network stability.

### Potential Security Risk: Rogue Root Bridge

An unauthorized switch may send superior BPDUs and attempt to become the Root Bridge.

This can change traffic paths and potentially affect network availability.

### Security Best Practices

- Select a trusted Root Bridge.
- Configure Root Guard on appropriate designated ports.
- Use BPDU Guard on suitable edge ports.
- Monitor STP topology changes.
- Restrict physical access to network switches.
- Review unexpected root-inconsistent ports.
- Document switch connections and STP design.

---

# 13. Key Takeaways

- Root Guard protects the intended Root Bridge election.
- It prevents a downstream switch from becoming Root Bridge through a protected port.
- It monitors incoming superior BPDUs.
- A protected port enters root-inconsistent state when superior BPDUs are received.
- The port automatically recovers when superior BPDUs stop.
- Root Guard is generally configured on designated ports facing downstream switches.
- It helps improve Layer 2 network stability and security.

---

# 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco 2960 Switch
- Cisco IOS CLI
- Ethernet Networking

---

# 📚 What I Learned Today

Today I learned how Root Guard protects the Root Bridge election in a switched network.

I explored superior BPDUs, root-inconsistent states, Root Guard configuration, verification commands, and practical switch topology design.

This helped me understand how to protect Layer 2 networks from unexpected STP topology changes.

---

## 🚀 60 Days Networking Journey

**DAY 24/60 — ROOT GUARD (STP TOOLKIT) COMPLETED ✅**

LEARN → PRACTICE → DOCUMENT → IMPROVE

#Networking #RootGuard #STP #CiscoPacketTracer #CCNA #NetworkSecurity #CyberSecurity #Switching #LearningInPublic
