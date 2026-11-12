
# BPDU Guard & BPDU Filter (STP Toolkit) | Day 23
## 60 Days Networking Journey

## 📌 Overview

Today I learned about two important STP features:

- **BPDU Guard:** Protects PortFast-enabled edge ports by disabling a port if it receives a BPDU.
- **BPDU Filter:** Suppresses the sending and receiving of BPDUs under specific configurations.

Both features are related to Spanning Tree Protocol (STP), but they work differently.

---

## 🎯 Learning Objectives

- Understand BPDU Guard.
- Understand BPDU Filter.
- Learn why BPDUs are important.
- Configure BPDU Guard on access ports.
- Configure BPDU Filter.
- Understand interface-level and global configurations.
- Verify configurations using Cisco IOS commands.
- Practice both features in Cisco Packet Tracer.
- Understand STP security risks.

---

# 1. What is a BPDU?

BPDU stands for **Bridge Protocol Data Unit**.

It is a control message exchanged between switches to share Spanning Tree Protocol information.

BPDUs help switches:

- Elect the Root Bridge.
- Identify network topology.
- Select Root and Designated Ports.
- Prevent Layer 2 switching loops.
- Detect topology changes.

### Why are BPDUs Important?

Without BPDU information, switches cannot properly exchange STP information.

BPDUs help maintain a loop-free Layer 2 network.

---

# 2. What is BPDU Guard?

BPDU Guard is a Cisco STP security feature.

It protects edge ports, especially PortFast-enabled ports, from unexpected BPDU messages.

### How Does BPDU Guard Work?

1. A switch port is configured with PortFast and BPDU Guard.
2. The port connects to an end device.
3. The switch receives a BPDU on that port.
4. BPDU Guard detects the BPDU.
5. The switch places the interface into an error-disabled state.

### Why Do We Use BPDU Guard?

Imagine an employee connects an unauthorized switch to a network access port.

That switch may send BPDUs and potentially influence the STP topology.

BPDU Guard helps prevent this by disabling the affected port.

### Important Points

- BPDU Guard is commonly used on edge/access ports.
- It protects against unexpected BPDUs.
- A BPDU Guard violation can place the port into an error-disabled state.
- It does not replace STP.

---

# 3. BPDU Guard Configuration

## Method 1: Interface-Level Configuration

Enable BPDU Guard on a specific interface.

### Commands

```bash
enable
configure terminal

interface fastEthernet 0/1
switchport mode access
spanning-tree portfast
spanning-tree bpduguard enable

end
```

### Explanation

| Command | Purpose |
|---|---|
| `enable` | Enters privileged EXEC mode. |
| `configure terminal` | Enters global configuration mode. |
| `interface fastEthernet 0/1` | Selects the interface. |
| `switchport mode access` | Configures the port as an access port. |
| `spanning-tree portfast` | Enables PortFast. |
| `spanning-tree bpduguard enable` | Enables BPDU Guard on the interface. |
| `end` | Exits configuration mode. |

## Method 2: Global BPDU Guard Configuration

Enable BPDU Guard by default on PortFast-enabled ports.

```bash
configure terminal

spanning-tree portfast default
spanning-tree portfast bpduguard default

end
```

**Important:** Global BPDU Guard default applies to PortFast-enabled ports. An interface-level `spanning-tree bpduguard disable` can override the global default on supported platforms.

---

# 4. What Happens When BPDU Guard Detects a BPDU?

Suppose BPDU Guard is enabled on Fa0/1.

An unauthorized switch is connected to that interface.

The connected switch sends a BPDU.

BPDU Guard detects it and disables the port.

### Check Interface Status

```bash
show interfaces status
```

You may see a status such as:

```text
Fa0/1    disabled
```

Depending on the platform, error-disabled status may also be visible through other commands.

### Check Error-Disabled Interfaces

```bash
show interfaces status err-disabled
```

### Check Logs

```bash
show logging
```

---

# 5. Recover an Error-Disabled Port

First, investigate why the BPDU was received.

Do not restore the port until the unauthorized or unexpected connection has been addressed.

### Manual Recovery

```bash
configure terminal

interface fastEthernet 0/1
shutdown
no shutdown

end
```

### Automatic Recovery

Some Cisco switches support automatic recovery:

```bash
configure terminal

errdisable recovery cause bpduguard
errdisable recovery interval 300

end
```

This configures a recovery interval of 300 seconds.

**Security Note:** Automatic recovery does not remove the original cause of the violation. Use it only when appropriate.

---

# 6. What is BPDU Filter?

BPDU Filter is a Cisco STP feature that suppresses BPDU transmission and reception under specific configurations.

It is more sensitive than BPDU Guard because suppressing BPDUs can interfere with STP's ability to detect and prevent Layer 2 loops.

### Why is BPDU Filter Used?

It may be used in carefully designed network environments where BPDU exchange on a specific edge port is intentionally suppressed.

However, it should not be enabled casually.

### Important Difference

- BPDU Guard detects unexpected BPDUs and disables the port.
- BPDU Filter suppresses BPDU exchange.

BPDU Guard is generally a protective feature for edge ports.

BPDU Filter can weaken STP protection if used incorrectly.

---

# 7. BPDU Filter Configuration

There are two common configuration methods.

## Method 1: Interface-Level BPDU Filter

```bash
configure terminal

interface fastEthernet 0/1
spanning-tree portfast
spanning-tree bpdufilter enable

end
```

### Explanation

- `interface fastEthernet 0/1` selects the interface.
- `spanning-tree portfast` enables PortFast.
- `spanning-tree bpdufilter enable` enables BPDU filtering on that interface.

**Warning:** Interface-level BPDU Filter suppresses BPDU transmission and reception on the interface. This can prevent the switch from detecting an STP loop involving that port.

Use only when the topology and security requirements are fully understood.

## Method 2: Global BPDU Filter Configuration

```bash
configure terminal

spanning-tree portfast default
spanning-tree portfast bpdufilter default

end
```

### How Does Global BPDU Filter Work?

On supported Cisco implementations, global BPDU Filter default applies to PortFast-enabled ports.

- The port initially suppresses BPDU transmission.
- If the port receives a BPDU, filtering is disabled for that port and normal STP participation resumes.

This behavior differs from interface-level BPDU Filter.

---

# 8. BPDU Guard vs BPDU Filter

| Feature | BPDU Guard | BPDU Filter |
|---|---|---|
| Main Purpose | Protect edge ports | Suppress BPDU exchange |
| BPDU Received | Port is disabled | Behavior depends on configuration mode |
| STP Protection | Helps protect edge ports | Can weaken STP protection |
| Common Use | PortFast access ports | Carefully controlled edge scenarios |
| Security Risk | Can interrupt an incorrectly connected port | Can hide STP information and contribute to loops |

### Which One Should We Prefer?

For normal end-device access ports:

**PortFast + BPDU Guard** is generally the safer recommended approach.

BPDU Filter should only be used when there is a specific, understood requirement.

---

# 9. Cisco Packet Tracer Practical Lab

## Lab Objective

Configure PortFast and BPDU Guard, observe BPDU Guard behavior, and explore BPDU Filter configuration.

### Required Devices

- 2 × Cisco 2960 Switches
- 2 × PCs
- Ethernet cables

### Topology

```text
PC1 -------- SW1 -------- PC2
               |
             Fa0/24
               |
              SW2
```

Connections:

| Device | Interface | Connected To |
|---|---|---|
| PC1 | FastEthernet0 | SW1 Fa0/1 |
| PC2 | FastEthernet0 | SW1 Fa0/2 |
| SW1 | Fa0/24 | SW2 Fa0/24 |

---

## Part A: Configure PortFast and BPDU Guard

On SW1:

```bash
enable
configure terminal

interface fastEthernet 0/1
switchport mode access
spanning-tree portfast
spanning-tree bpduguard enable

end
```

Repeat on the PC-connected Fa0/2 interface if required.

## Part B: Verify BPDU Guard

```bash
show running-config
show spanning-tree interface fastEthernet 0/1 detail
show logging
```

## Part C: Test BPDU Guard

1. Connect an authorized PC to Fa0/1.
2. Confirm normal connectivity.
3. Disconnect the PC.
4. Connect another switch to Fa0/1.
5. Observe the BPDU Guard response if the connected switch sends BPDUs.

**Expected Result:** If a BPDU is received on the BPDU Guard-enabled interface, the interface should enter an error-disabled state.

Do this only in your isolated Packet Tracer lab.

## Part D: Explore BPDU Filter

On an isolated test interface:

```bash
configure terminal

interface fastEthernet 0/3
switchport mode access
spanning-tree portfast
spanning-tree bpdufilter enable

end
```

Verify:

```bash
show running-config
show spanning-tree interface fastEthernet 0/3 detail
```

Observe the available output and compare the configuration with BPDU Guard.

**Note:** Packet Tracer support and behavior can vary by switch model. If a command is rejected, check the model's supported IOS commands. Do not assume a rejected command means the concept is incorrect.

---

# 10. Important Verification Commands

| Command | Purpose |
|---|---|
| `show spanning-tree` | Displays STP information. |
| `show spanning-tree summary` | Displays STP summary. |
| `show spanning-tree interface fa0/1 detail` | Displays detailed STP interface information. |
| `show running-config` | Displays current configuration. |
| `show interfaces status` | Displays switch interface status. |
| `show interfaces status err-disabled` | Displays error-disabled interfaces, if supported. |
| `show logging` | Displays system log messages. |
| `show errdisable recovery` | Displays error-disabled recovery settings. |

---

# 11. Troubleshooting

| Problem | Possible Solution |
|---|---|
| Port becomes error-disabled | Check BPDU Guard and identify the source of BPDUs. |
| PC loses connectivity | Check port status and VLAN configuration. |
| BPDU Filter command rejected | Check switch model and IOS support. |
| STP information is missing | Verify STP configuration and VLAN status. |
| Port does not recover | Investigate the cause, then use manual or configured recovery. |

---

# 12. Cybersecurity Connection 🔐

BPDU Guard and BPDU Filter are important in Layer 2 network security.

### BPDU Guard Security Benefits

- Helps prevent unauthorized switches from influencing STP through protected edge ports.
- Protects the intended edge-port design.
- Helps reduce the risk of unexpected STP topology changes.

### BPDU Filter Security Risks

- Suppresses important STP control information.
- Can prevent the switch from detecting a loop through a filtered interface.
- Incorrect use can contribute to broadcast storms and network instability.

### Best Practices

- Enable PortFast on suitable end-device access ports.
- Enable BPDU Guard on edge ports.
- Avoid unnecessary BPDU Filter configuration.
- Never blindly filter BPDUs on switch-to-switch links.
- Monitor error-disabled ports and switch logs.
- Keep STP enabled across the switching infrastructure.

---

# 13. Key Takeaways

- BPDU stands for Bridge Protocol Data Unit.
- BPDUs carry STP control information.
- BPDU Guard disables a protected port when it receives a BPDU.
- BPDU Filter suppresses BPDU exchange.
- Interface-level and global BPDU Filter configurations behave differently.
- BPDU Guard is generally preferred for protecting PortFast edge ports.
- Incorrect BPDU Filter usage can create Layer 2 security and availability risks.

---

# 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco 2960 Switch
- Cisco IOS CLI
- Ethernet Networking

---

# 📚 What I Learned Today

Today I learned how BPDU Guard and BPDU Filter work as part of the STP toolkit.

I practiced their configurations, explored the behavior of protected edge ports, and understood why BPDU Guard is commonly recommended for end-device access ports.

I also learned that BPDU Filter must be used carefully because suppressing STP messages can weaken loop prevention.

---

## 🚀 60 Days Networking Journey

**DAY 23/60 — BPDU GUARD & BPDU FILTER COMPLETED ✅**

LEARN → PRACTICE → DOCUMENT → IMPROVE

#Networking #BPDUGuard #BPDUFilter #STP #CiscoPacketTracer #CCNA #NetworkSecurity #CyberSecurity #LearningInPublic
