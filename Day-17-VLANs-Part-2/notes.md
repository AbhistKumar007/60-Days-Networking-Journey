
# Day 17 — VLANs (Part 2) | Practical Lab 🌐

## 📌 Topic

**VLAN Trunking & Inter-VLAN Routing**

Today I continued learning about VLANs and practiced configuring trunk ports and Inter-VLAN Routing using Cisco Packet Tracer.

In Day 16, I learned how VLANs logically separate devices within a network.

Today, I explored how multiple VLANs can communicate across switches and how routing enables communication between different VLANs.

---

## 🎯 Learning Objectives

- Understand VLAN Trunking
- Differentiate between Access and Trunk ports
- Understand IEEE 802.1Q
- Configure trunk interfaces
- Understand Inter-VLAN Routing
- Configure Router-on-a-Stick
- Verify VLAN connectivity
- Troubleshoot common VLAN issues.

---

# 1. What is VLAN Trunking?

VLAN Trunking allows multiple VLANs to communicate across a single physical connection between network devices.

A trunk link carries traffic belonging to multiple VLANs.

### Example

```text
        SWITCH 1                  SWITCH 2
       ┌─────────┐               ┌─────────┐
PC1 ───│ VLAN 10 │               │ VLAN 10 │─── PC3
       │         │──── TRUNK ────│         │
PC2 ───│ VLAN 20 │               │ VLAN 20 │─── PC4
       └─────────┘               └─────────┘
```

The trunk connection carries traffic from both VLAN 10 and VLAN 20.

---

# 2. Access Port vs Trunk Port

| Feature | Access Port | Trunk Port |
|---|---|---|
| Purpose | Connects end devices | Connects VLAN-aware network devices |
| VLAN Traffic | Normally one data VLAN | Multiple VLANs |
| VLAN Tagging | Normally untagged | Uses IEEE 802.1Q tagging |
| Example | PC to Switch | Switch to Switch |

---

# 3. IEEE 802.1Q

IEEE 802.1Q is a standard used to identify VLAN traffic on Ethernet trunk links.

It inserts a VLAN tag into an Ethernet frame.

This tag helps the receiving switch identify which VLAN the frame belongs to.

### Example

```text
Ethernet Frame
      |
      ▼
802.1Q VLAN Tag
      |
      ▼
VLAN ID = 10
```

**Important:** Native VLAN traffic is normally sent untagged on an 802.1Q trunk unless configured otherwise.

---

# 4. What is Inter-VLAN Routing?

By default, different VLANs are separate Layer 2 broadcast domains.

Devices in different VLANs require a Layer 3 device to communicate.

This process is called **Inter-VLAN Routing**.

Common methods include:

1. Router-on-a-Stick
2. Multilayer Switch using SVIs
3. Legacy routing using separate physical router interfaces

---

# 5. Router-on-a-Stick

Router-on-a-Stick allows multiple VLANs to communicate using a single physical router interface.

The router interface is divided into logical subinterfaces.

Each subinterface is configured for a specific VLAN.

### Example

```text
                ROUTER
                  |
           GigabitEthernet0/0
                  |
               TRUNK
                  |
                SWITCH
               /      \
              /        \
          VLAN 10     VLAN 20
             |           |
            PC1         PC2
```

---

# 💻 Practical Lab — Cisco Packet Tracer

## 6. Lab Requirements

### Devices Required

- 1 Router (Cisco 2911)
- 2 Cisco 2960 Switches
- 4 PCs
- Copper straight-through cables

### Network Topology

```text
                       ROUTER
                         |
                         | Trunk
                         |
                      SWITCH 1
                     /   |    \
                    /    |     \
                 PC1    PC2    SWITCH 2
                                  |
                               /     \
                              /       \
                            PC3       PC4
```

### VLAN Assignment

```text
VLAN 10 → HR Department
VLAN 20 → IT Department
```

The router connects to Switch 1 through a trunk link.

Switch 1 and Switch 2 are also connected through a trunk link.

---

# 7. IP Addressing Table

| Device | IP Address | Subnet Mask | VLAN | Gateway |
|---|---|---|---|---|
| PC1 | 192.168.10.10 | 255.255.255.0 | 10 | 192.168.10.1 |
| PC2 | 192.168.20.10 | 255.255.255.0 | 20 | 192.168.20.1 |
| PC3 | 192.168.10.20 | 255.255.255.0 | 10 | 192.168.10.1 |
| PC4 | 192.168.20.20 | 255.255.255.0 | 20 | 192.168.20.1 |

---

# 8. Step 1 — Create VLANs

Perform this configuration on both switches.

```cisco
enable
configure terminal

vlan 10
name HR
exit

vlan 20
name IT
exit
```

### Explanation

- `vlan 10` creates VLAN 10.
- `name HR` assigns the VLAN name.
- `vlan 20` creates VLAN 20.
- `name IT` assigns the second VLAN name.

---

# 9. Step 2 — Configure Access Ports

Assume the following connections:

### Switch 1

- PC1 → Fa0/1
- PC2 → Fa0/2

Configuration:

```cisco
interface fastEthernet 0/1
switchport mode access
switchport access vlan 10
exit

interface fastEthernet 0/2
switchport mode access
switchport access vlan 20
exit
```

### Switch 2

- PC3 → Fa0/1
- PC4 → Fa0/2

Configuration:

```cisco
interface fastEthernet 0/1
switchport mode access
switchport access vlan 10
exit

interface fastEthernet 0/2
switchport mode access
switchport access vlan 20
exit
```

---

# 10. Step 3 — Configure Trunk Ports

### Switch 1

Assume:

- Gi0/1 connects to Switch 2.
- Gi0/2 connects to the Router.

```cisco
interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 10,20
exit

interface gigabitEthernet 0/2
switchport mode trunk
switchport trunk allowed vlan 10,20
exit
```

### Switch 2

Assume Gi0/1 connects to Switch 1.

```cisco
interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 10,20
exit
```

### Verify Trunk Configuration

```cisco
end
show interfaces trunk
```

This command displays operational trunk interfaces and their allowed VLANs.

---

# 11. Step 4 — Configure Router-on-a-Stick

Open the Router CLI.

Enter configuration mode:

```cisco
enable
configure terminal
```

### Enable the Physical Interface

```cisco
interface gigabitEthernet 0/0
no shutdown
exit
```

### Configure VLAN 10 Subinterface

```cisco
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit
```

### Configure VLAN 20 Subinterface

```cisco
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit
```

### Explanation

| Command | Purpose |
|---|---|
| `interface g0/0.10` | Creates a logical subinterface |
| `encapsulation dot1Q 10` | Associates it with VLAN 10 |
| `ip address` | Assigns the VLAN gateway |
| `no shutdown` | Enables the physical interface |

**Note:** Interface names may differ depending on the router model used in Packet Tracer.

---

# 12. Step 5 — Configure Default Gateways

Configure the following gateways on the PCs:

```text
PC1 → 192.168.10.1
PC2 → 192.168.20.1
PC3 → 192.168.10.1
PC4 → 192.168.20.1
```

The default gateway allows a device to send traffic outside its local subnet.

---

# 13. Step 6 — Verify Configuration

### On the Switches

Check VLAN assignments:

```cisco
show vlan brief
```

Check trunk interfaces:

```cisco
show interfaces trunk
```

### On the Router

Check interface status:

```cisco
show ip interface brief
```

Check routing information:

```cisco
show ip route
```

The routing table should contain connected routes for both VLAN networks when the interfaces are operational.

---

# 14. Step 7 — Test Connectivity

Open PC1's Command Prompt.

### Test 1 — Same VLAN

```bash
ping 192.168.10.20
```

PC1 → PC3

Both devices belong to VLAN 10.

Expected result: Successful communication.

### Test 2 — Different VLAN

```bash
ping 192.168.20.10
```

PC1 → PC2

These devices belong to different VLANs.

Expected result: Successful communication through Inter-VLAN Routing when all configurations are correct.

### Test 3 — Different VLAN Across Switches

```bash
ping 192.168.20.20
```

PC1 → PC4

Traffic travels through the router and trunk links to reach the destination.

**Note:** The first ping may occasionally time out while ARP resolution takes place.

---

# 15. Understanding Packet Flow

When PC1 sends data to PC4:

```text
PC1 (VLAN 10)
      |
      ▼
   Switch 1
      |
      ▼
Router Subinterface
      |
      ▼
Inter-VLAN Routing
      |
      ▼
   Switch 1
      |
      ▼
  Trunk Link
      |
      ▼
   Switch 2
      |
      ▼
PC4 (VLAN 20)
```

The router processes the packet at Layer 3 and forwards it toward the destination VLAN.

---

# 16. Useful Cisco Commands

| Command | Purpose |
|---|---|
| `show vlan brief` | Displays VLAN information |
| `show interfaces trunk` | Displays trunk interfaces |
| `show ip interface brief` | Displays interface IPs and status |
| `show ip route` | Displays the routing table |
| `show running-config` | Displays current configuration |
| `show mac address-table` | Displays learned MAC addresses |
| `ping` | Tests connectivity |
| `copy running-config startup-config` | Saves configuration |

---

# 17. Common Troubleshooting Problems

If Inter-VLAN communication fails, check:

- Are the VLANs created on both switches?
- Are access ports assigned correctly?
- Are trunk ports configured properly?
- Are VLAN 10 and VLAN 20 allowed on the trunks?
- Is the router's physical interface enabled?
- Are router subinterfaces configured correctly?
- Are the 802.1Q VLAN IDs correct?
- Are PC IP addresses and subnet masks correct?
- Are default gateways configured correctly?

### Troubleshooting Commands

```cisco
show vlan brief
show interfaces trunk
show ip interface brief
show ip route
show running-config
```

---

# 🔐 18. Cybersecurity Connection

Understanding VLAN Trunking and Inter-VLAN Routing is important for network security.

### Applications

- Network segmentation
- Department-based isolation
- Guest network separation
- Traffic monitoring
- Firewall integration
- Access Control Lists (ACLs)
- Secure enterprise network design

### Security Considerations

- Restrict unnecessary VLANs on trunk ports.
- Disable unused switch interfaces.
- Avoid unnecessary automatic trunk negotiation.
- Use ACLs or firewalls to control Inter-VLAN communication.
- Keep management traffic separated where appropriate.

**Important:** Inter-VLAN Routing enables connectivity, but additional security policies are required to restrict which VLANs can communicate.

---

# 🧠 19. Key Takeaways

```text
VLAN
  ↓
Logical Network Separation
  ↓
Trunking
  ↓
Multiple VLANs Across Switches
  ↓
Router-on-a-Stick
  ↓
Inter-VLAN Communication
```

### What I Learned

- Access ports normally connect end devices.
- Trunk ports carry traffic for multiple VLANs.
- IEEE 802.1Q identifies VLAN traffic.
- Different VLANs require Layer 3 routing to communicate.
- Router-on-a-Stick uses router subinterfaces.
- `show interfaces trunk` helps verify trunk configuration.
- Correct IP addressing and gateways are essential.

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- Cisco 2960 Switch
- Cisco Router
- IPv4
- Ping
- Simulation Mode

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 17/60 — VLANs (Part 2) Lab Completed ✅**

LEARN → PRACTICE → DOCUMENT → IMPROVE
