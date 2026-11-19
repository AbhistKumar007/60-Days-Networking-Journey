
# Day 25 — Loop Guard (STP Toolkit)

## 1. Introduction to Loop Guard

Loop Guard is a Spanning Tree Protocol (STP) security feature that helps prevent Layer 2 switching loops caused by the unexpected loss of Bridge Protocol Data Units (BPDUs).

It protects STP from incorrectly transitioning a port into the Designated or Forwarding state when BPDUs are no longer received on a port that should continue receiving them.

### Why do we need Loop Guard?

STP uses BPDUs to exchange information between switches and maintain a loop-free network.

Imagine:
- Switch A is the Root Bridge.
- Switch B receives BPDUs from Switch A.
- The link experiences a problem.
- Switch B suddenly stops receiving BPDUs.

Without Loop Guard, the port might incorrectly assume that it has become the Designated Port and move to the Forwarding state.

This can create a Layer 2 loop.

Loop Guard helps prevent this situation.

---

## 2. What is a BPDU?

BPDU stands for Bridge Protocol Data Unit.

It is a control frame exchanged between switches running STP.

BPDUs help switches:
- Elect the Root Bridge.
- Calculate the best path to the Root Bridge.
- Select Root and Designated Ports.
- Detect changes in network topology.
- Prevent switching loops.

**Important:** STP depends on receiving BPDUs to maintain its topology.

---

## 3. How Does Loop Guard Work?

Loop Guard monitors ports that are expected to receive BPDUs.

If a port stops receiving expected BPDUs, Loop Guard places it into a loop-inconsistent state.

This prevents the port from incorrectly forwarding traffic.

When valid BPDUs start arriving again, the port automatically recovers from the loop-inconsistent state.

### Working process

1. Switch receives BPDUs on a port.
2. STP identifies the port's role and state.
3. The port unexpectedly stops receiving BPDUs.
4. Loop Guard detects the missing BPDUs.
5. The port enters a loop-inconsistent state.
6. The port does not forward normal traffic.
7. When BPDUs return, the port automatically recovers.

---

## 4. Loop-Inconsistent State

A loop-inconsistent state is a protective STP condition.

When Loop Guard detects that expected BPDUs are missing:
- The port is prevented from forwarding traffic.
- The port does not become an active forwarding path.
- STP continues protecting the network from a potential loop.

**Important:** Loop-inconsistent is not the same as error-disabled.

The port can recover automatically when the expected BPDUs return.

---

## 5. Loop Guard Configuration

### Enable Loop Guard on a Specific Interface

```bash
enable
configure terminal

interface fastEthernet 0/1
spanning-tree guard loop

end
```

### Explanation

| Command | Purpose |
|---|---|
| `enable` | Enters privileged EXEC mode |
| `configure terminal` | Enters global configuration mode |
| `interface fastEthernet 0/1` | Selects the interface |
| `spanning-tree guard loop` | Enables Loop Guard |
| `end` | Returns to privileged EXEC mode |

### Enable Loop Guard Globally

```bash
configure terminal
spanning-tree loopguard default
end
```

This enables Loop Guard by default on eligible spanning-tree ports, subject to platform and STP implementation behavior.

---

## 6. Verify Loop Guard Configuration

Use these commands to inspect the STP configuration and port status.

### Check STP Information

```bash
show spanning-tree
```

Displays:
- Root Bridge information.
- Port roles.
- Port states.
- Path costs.

### Check Loop-Inconsistent Ports

```bash
show spanning-tree inconsistentports
```

Displays ports currently in an STP inconsistent state.

### Check a Specific Interface

```bash
show spanning-tree interface fastEthernet 0/1 detail
```

Displays detailed STP information for the selected interface.

### Check Running Configuration

```bash
show running-config
```

Displays the current device configuration.

---

## 7. Practical Lab — Loop Guard

### Objective

Understand how Loop Guard protects a switched network when expected BPDUs stop arriving.

### Required Devices

- 3 Cisco switches.
- Copper Ethernet cables.
- Cisco Packet Tracer.

### Network Topology

```text
        SW1
       /   \
      /     \
    SW2-----SW3
```

This triangle topology provides redundant Layer 2 paths.

### Step 1 — Connect the Switches

Connect:
- SW1 to SW2.
- SW1 to SW3.
- SW2 to SW3.

Use the available Gigabit Ethernet interfaces.

### Step 2 — Configure STP

On all switches:

```bash
enable
configure terminal
spanning-tree mode pvst
end
```

### Step 3 — Select the Root Bridge

On SW1:

```bash
configure terminal
spanning-tree vlan 1 priority 4096
end
```

Verify:

```bash
show spanning-tree vlan 1
```

SW1 should become the Root Bridge if its Bridge ID is the lowest.

### Step 4 — Enable Loop Guard

On an eligible port of SW2 that receives BPDUs from the root-side path:

```bash
configure terminal
interface gigabitEthernet 0/1
spanning-tree guard loop
end
```

Use the actual interface connected to the intended STP neighbor.

### Step 5 — Verify Configuration

```bash
show spanning-tree interface gigabitEthernet 0/1 detail
show spanning-tree inconsistentports
show spanning-tree vlan 1
```

### Step 6 — Understand BPDU Loss

In a controlled lab, investigate how the port behaves if expected BPDUs stop arriving.

A port protected by Loop Guard should enter the loop-inconsistent state instead of incorrectly becoming a forwarding port.

**Note:** Packet Tracer may not reproduce every BPDU-loss condition or Loop Guard behavior. If the feature is unsupported in your Packet Tracer version, use a supported Cisco IOS lab environment to verify it.

---

## 8. Loop Guard vs Root Guard vs BPDU Guard

| Feature | Purpose | Trigger |
|---|---|---|
| Loop Guard | Prevents loops caused by missing expected BPDUs | Expected BPDUs stop arriving |
| Root Guard | Prevents an unexpected switch from becoming Root Bridge through a protected port | Superior BPDU received |
| BPDU Guard | Protects edge ports from unexpected BPDUs | Any BPDU received on a protected port |
| BPDU Filter | Suppresses BPDU transmission/reception in configured cases | BPDU filtering configuration |

### Important Difference

- Root Guard protects the Root Bridge election.
- BPDU Guard protects edge ports.
- Loop Guard protects against unexpected BPDU loss.
- BPDU Filter can weaken STP protection if used incorrectly.

---

## 9. Loop Guard and Network Security

Loop Guard is important for network reliability and Layer 2 security.

It helps:
- Prevent Layer 2 switching loops.
- Reduce the risk of broadcast storms.
- Maintain a stable STP topology.
- Protect redundant network paths.
- Reduce network outages caused by unexpected BPDU loss.

### Best Practices

1. Enable Loop Guard on appropriate redundant STP links.
2. Use Root Guard where downstream switches must not become Root Bridge.
3. Use BPDU Guard on PortFast edge ports.
4. Avoid unnecessary BPDU filtering.
5. Regularly monitor STP port states.
6. Document switch topology and STP configuration.

---

## 10. Important Commands Cheat Sheet

| Command | Purpose |
|---|---|
| `spanning-tree guard loop` | Enables Loop Guard on an interface |
| `spanning-tree loopguard default` | Enables Loop Guard globally |
| `show spanning-tree` | Displays STP information |
| `show spanning-tree vlan 1` | Displays STP information for VLAN 1 |
| `show spanning-tree inconsistentports` | Displays inconsistent ports |
| `show spanning-tree interface fa0/1 detail` | Displays interface-level STP details |
| `show running-config` | Displays current configuration |

---

## 11. Key Takeaways

- Loop Guard is an STP protection feature.
- It protects against unexpected loss of BPDUs.
- It prevents an affected port from incorrectly becoming a forwarding port.
- A protected port can enter the loop-inconsistent state.
- It automatically recovers when expected BPDUs return.
- It is especially useful in redundant Layer 2 network topologies.

## 12. Learning Progress

- [x] Understand STP fundamentals.
- [x] Learn Root Bridge election.
- [x] Understand PortFast.
- [x] Learn BPDU Guard and BPDU Filter.
- [x] Learn Root Guard.
- [x] Learn Loop Guard.
- [ ] Practice Loop Guard in a supported lab environment.
- [ ] Study advanced STP protection features.

---

## Next Topic

**UDLD — Unidirectional Link Detection**

Learn how switches detect unidirectional links and protect Layer 2 networks from problems caused by one-way communication.

---

**LEARN → PRACTICE → DOCUMENT → IMPROVE**

#Networking #CiscoPacketTracer #STP #LoopGuard #NetworkSecurity #CCNA #CyberSecurity #60DaysNetworkingJourney
