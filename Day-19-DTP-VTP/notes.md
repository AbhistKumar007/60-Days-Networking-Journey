
# DTP & VTP | Day 19 — 60 Days Networking Journey

## 📌 Overview

Today I learned about two important Cisco switching protocols:

- **DTP (Dynamic Trunking Protocol):** Automatically negotiates trunk links between switches.
- **VTP (VLAN Trunking Protocol):** Shares VLAN information between switches in the same VTP domain.

Both protocols help manage VLANs and trunk connections in a switched network.

---

## 🎯 Learning Objectives

- Understand DTP and VTP.
- Learn the difference between access and trunk ports.
- Understand DTP negotiation modes.
- Learn VTP operating modes.
- Configure DTP and VTP using Cisco IOS commands.
- Verify configurations using show commands.
- Understand security risks and best practices.

---

# 1. Dynamic Trunking Protocol (DTP)

## What is DTP?

DTP stands for **Dynamic Trunking Protocol**.

It is a Cisco-proprietary protocol that allows switches to negotiate whether a link should operate as a trunk link.

A trunk link carries traffic from multiple VLANs between switches using VLAN tagging.

### Why is DTP used?

Without manual configuration, compatible Cisco switches can use DTP to negotiate trunk connections.

### DTP Modes

| Mode | Description |
|---|---|
| Access | Forces the port to operate as an access port. |
| Trunk | Forces the port to operate as a trunk port. |
| Dynamic Auto | Waits for the other switch to initiate trunk negotiation. |
| Dynamic Desirable | Actively attempts to form a trunk. |

### DTP Negotiation

| Switch 1 | Switch 2 | Result |
|---|---|---|
| Dynamic Desirable | Dynamic Auto | Trunk |
| Dynamic Desirable | Dynamic Desirable | Trunk |
| Dynamic Auto | Dynamic Auto | Access |
| Trunk | Trunk | Trunk |
| Access | Access | Access |

**Important:** Exact behavior depends on platform and configuration. DTP must be supported and enabled for dynamic negotiation to work.

---

## DTP Configuration

### Step 1: Enter privileged mode

```bash
enable
configure terminal
```

### Step 2: Select the interface

```bash
interface gigabitEthernet 0/1
```

### Step 3: Configure the port as a trunk

```bash
switchport mode trunk
```

### Step 4: Disable DTP negotiation

```bash
switchport nonegotiate
```

This prevents the interface from sending DTP negotiation frames.

**Note:** Use `switchport nonegotiate` with a statically configured trunk port, not a dynamically negotiated port.

### Step 5: Verify the configuration

```bash
show interfaces trunk
show interfaces gigabitEthernet 0/1 switchport
```

---

# 2. VLAN Trunking Protocol (VTP)

## What is VTP?

VTP stands for **VLAN Trunking Protocol**.

It is a Cisco-proprietary protocol that distributes VLAN configuration information between switches in the same VTP domain.

Instead of manually creating the same VLANs on every switch, VTP can distribute VLAN information from a VTP server to other participating switches.

### Why is VTP used?

- Centralized VLAN management.
- Reduces repetitive VLAN configuration.
- Helps maintain consistent VLAN information.
- Useful in networks containing multiple switches.

### VTP Requirements

For VTP to exchange VLAN information successfully, switches generally need:

- The same VTP domain name.
- Compatible VTP versions.
- Compatible authentication settings, if configured.
- Working trunk links between switches.
- A supported VTP operating mode.

---

## VTP Modes

| Mode | Description |
|---|---|
| Server | Can create, modify, and delete VLANs and advertise VLAN information. |
| Client | Learns VLAN information and cannot independently create or modify normal VLANs. |
| Transparent | Maintains its own VLAN configuration and forwards VTP advertisements in VTP versions 1 and 2. |

**Note:** VTP version 3 has additional capabilities and different role behavior.

---

## VTP Configuration

### Step 1: Enter configuration mode

```bash
enable
configure terminal
```

### Step 2: Configure the VTP domain

```bash
vtp domain NETWORK
```

### Step 3: Configure VTP version

```bash
vtp version 2
```

### Step 4: Configure the switch as a server

```bash
vtp mode server
```

### Step 5: Configure a VTP password (optional)

```bash
vtp password MyVTPPassword123
```

Use the same password on participating switches when password authentication is configured.

### Step 6: Verify VTP configuration

```bash
show vtp status
show vlan brief
```

---

# 3. Practical Lab — DTP & VTP in Cisco Packet Tracer

## Lab Topology

Create a simple network with:

- 2 Cisco 2960 switches.
- 2 PCs.
- Ethernet cables.

### Connections

| Device | Interface | Connected To |
|---|---|---|
| PC1 | FastEthernet0 | Switch1 Fa0/1 |
| Switch1 | Fa0/24 | Switch2 Fa0/24 |
| PC2 | FastEthernet0 | Switch2 Fa0/1 |

### IP Addressing

| Device | IP Address | Subnet Mask |
|---|---|---|
| PC1 | 192.168.10.10 | 255.255.255.0 |
| PC2 | 192.168.10.20 | 255.255.255.0 |

## Part A: Configure VTP

### Switch 1 — VTP Server

```bash
enable
configure terminal

vtp domain NETWORK
vtp version 2
vtp mode server

vlan 10
name HR

vlan 20
name IT

end
```

### Switch 2 — VTP Client

```bash
enable
configure terminal

vtp domain NETWORK
vtp version 2
vtp mode client

end
```

Check whether VLANs 10 and 20 appear on Switch 2:

```bash
show vlan brief
show vtp status
```

## Part B: Configure the trunk link

Configure Switch 1:

```bash
enable
configure terminal

interface fastEthernet 0/24
switchport mode trunk
switchport trunk allowed vlan 10,20

end
```

Configure Switch 2:

```bash
enable
configure terminal

interface fastEthernet 0/24
switchport mode trunk
switchport trunk allowed vlan 10,20

end
```

## Part C: Assign access ports

Switch 1:

```bash
configure terminal

interface fastEthernet 0/1
switchport mode access
switchport access vlan 10

end
```

Switch 2:

```bash
configure terminal

interface fastEthernet 0/1
switchport mode access
switchport access vlan 10

end
```

## Part D: Test Connectivity

Configure the PCs with the IP addresses shown above.

From PC1, run:

```bash
ping 192.168.10.20
```

If the VLAN configuration and trunk link are correct, both PCs should communicate through VLAN 10.

---

# 4. Important Verification Commands

| Command | Purpose |
|---|---|
| `show vlan brief` | Displays VLAN information. |
| `show interfaces trunk` | Displays active trunk interfaces. |
| `show interfaces switchport` | Displays switchport configuration. |
| `show vtp status` | Displays VTP information. |
| `show running-config` | Displays current configuration. |
| `show interfaces status` | Displays port status. |

Some commands, including DTP-specific verification commands, may not be supported by every Packet Tracer switch model.

---

# 5. Troubleshooting

| Problem | Possible Solution |
|---|---|
| VLANs are not appearing | Check VTP domain, version, mode, and status. |
| Trunk is not forming | Check trunk configuration on both switches. |
| PCs cannot communicate | Verify IP addresses, VLAN assignments, and port status. |
| VTP updates are not received | Check trunk connectivity and VTP compatibility. |
| VLAN traffic is blocked | Check allowed VLANs on the trunk. |

---

# 6. Cybersecurity Connection 🔐

DTP and VTP are important from a network security perspective.

### DTP Security Risks

If trunk negotiation is enabled on an unnecessary or exposed port, an attacker with physical network access may attempt a VLAN hopping attack by negotiating a trunk.

**Best Practices:**
- Disable DTP negotiation on statically configured trunk ports.
- Configure unused ports as access ports.
- Shut down unused interfaces.
- Allow only required VLANs on trunk links.
- Avoid unnecessary trunk connections.

### VTP Security Risks

Incorrect VTP configuration can cause unwanted VLAN database changes across switches.

A switch with an inappropriate VTP configuration or revision number may affect VLAN information in the domain.

**Best Practices:**
- Use VTP only when required.
- Configure the correct VTP domain and version.
- Protect VTP with authentication where supported.
- Understand revision numbers before connecting switches.
- Consider VTP transparent or off mode where centralized VLAN distribution is unnecessary.

---

# 7. DTP vs VTP

| Feature | DTP | VTP |
|---|---|---|
| Full Form | Dynamic Trunking Protocol | VLAN Trunking Protocol |
| Purpose | Negotiates trunk links | Distributes VLAN information |
| Works With | Switch interfaces | Switches in a VTP domain |
| Main Function | Trunk negotiation | VLAN management |
| Protocol Type | Cisco proprietary | Cisco proprietary |

---

# 8. Key Takeaways

- DTP negotiates trunk connections between compatible Cisco switches.
- VTP distributes VLAN information within a VTP domain.
- Trunk links can carry traffic from multiple VLANs.
- VTP Server, Client, and Transparent modes have different responsibilities.
- Incorrect configurations can create connectivity and security problems.
- Network administrators should configure trunk ports and VLAN management carefully.

---

# 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- Cisco 2960 Switch
- Basic Ethernet Networking

---

# 📚 What I Learned Today

Today I learned how DTP negotiates trunk links and how VTP distributes VLAN information between switches. I also practiced configuring VTP modes, trunk ports, and VLAN assignments in Cisco Packet Tracer.

Understanding these protocols helps build a stronger foundation in switching, network administration, and network security.

---

## 🚀 60 Days Networking Journey

**DAY 19/60 — DTP & VTP COMPLETED ✅**

LEARN → PRACTICE → DOCUMENT → IMPROVE

#Networking #CiscoPacketTracer #DTP #VTP #VLAN #CCNA #NetworkSecurity #CyberSecurity
