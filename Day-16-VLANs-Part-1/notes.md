
# Day 16 — VLANs (Part 1) 🌐

## 📌 Topic: Virtual Local Area Networks (VLANs)

Today I learned about **VLANs (Virtual Local Area Networks)** and how they help divide a physical network into multiple logical networks.

I also practiced basic VLAN configuration using Cisco Packet Tracer.

---

## 🎯 Learning Objectives

- Understand what VLANs are
- Learn why VLANs are used
- Understand broadcast domains
- Learn about VLAN IDs
- Differentiate between access and trunk ports
- Create and configure VLANs
- Assign switch interfaces to VLANs
- Verify VLAN configurations

---

## 1. What is a VLAN?

A VLAN (Virtual Local Area Network) is a logical grouping of devices within a network.

It allows a single physical switch to support multiple separate logical networks.

### Example

Imagine an organization with three departments:

- HR Department
- IT Department
- Finance Department

Instead of using a separate physical switch for each department, we can create different VLANs on the same switch.

```text
              SWITCH
                 |
       ┌─────────┼─────────┐
       |         |         |
     VLAN 10   VLAN 20   VLAN 30
       HR        IT      FINANCE
```

Each VLAN forms a separate Layer 2 broadcast domain.

---

## 2. Why Do We Need VLANs?

VLANs provide several benefits:

| Benefit | Explanation |
|---|---|
| Segmentation | Divides a physical network into logical networks |
| Broadcast Control | Limits broadcasts to their respective VLANs |
| Management | Makes departmental network organization easier |
| Flexibility | Allows logical grouping independent of physical location |
| Security Support | Provides a foundation for traffic isolation and access policies |

**Important:** VLANs alone are not a complete security solution. Proper access controls and secure switch configurations are also required.

---

## 3. Understanding Broadcast Domains

A broadcast domain is a group of devices that receive each other's Layer 2 broadcast traffic.

Without VLAN segmentation, devices connected to a switch generally belong to the same broadcast domain.

With VLANs:

```text
VLAN 10 → Broadcast Domain 1

VLAN 20 → Broadcast Domain 2

VLAN 30 → Broadcast Domain 3
```

A broadcast generated in VLAN 10 is not automatically forwarded into VLAN 20.

---

## 4. VLAN IDs

Each VLAN is identified by a numerical ID.

Examples:

| VLAN ID | VLAN Name |
|---|---|
| 1 | Default |
| 10 | HR |
| 20 | IT |
| 30 | FINANCE |
| 99 | MANAGEMENT |

### Important Points

- VLAN 1 is the default VLAN on Cisco switches.
- Normal-range VLAN IDs are 1–1005.
- Extended-range VLAN IDs are 1006–4094.
- VLAN IDs 0 and 4095 are reserved.
- VLANs 1002–1005 are reserved for legacy technologies on Cisco switches.

---

## 5. Types of Switch Ports

### Access Port

An access port normally belongs to one VLAN and is commonly used to connect end devices.

Examples:

- Computers
- Printers
- IP phones (which may use an additional voice VLAN)

Configuration example:

```cisco
interface fastEthernet 0/1
switchport mode access
switchport access vlan 10
```

### Trunk Port

A trunk port carries traffic belonging to multiple VLANs.

It is commonly used between switches or between a switch and a VLAN-aware router.

IEEE 802.1Q is the standard tagging method commonly used on Ethernet trunks.

```text
        SWITCH 1
        VLAN 10
        VLAN 20
            |
            | TRUNK
            |
        SWITCH 2
        VLAN 10
        VLAN 20
```

### Access vs Trunk

| Access Port | Trunk Port |
|---|---|
| Normally carries one data VLAN | Carries multiple VLANs |
| Commonly connects end devices | Commonly connects network devices |
| Data traffic is normally untagged | Uses VLAN tagging for most carried VLANs |

---

# 💻 Practical Lab — Cisco Packet Tracer

## 6. Lab Objective

Create two VLANs on a Cisco switch and assign different PCs to them.

### Required Devices

- 1 Cisco 2960 Switch
- 4 PCs
- Copper straight-through cables

### Network Topology

```text
             SWITCH
          Cisco 2960
         /    /   \    \
        /    /     \    \
      PC1  PC2     PC3  PC4

      VLAN 10      VLAN 20
        HR            IT
```

---

## 7. IP Addressing Table

| Device | IP Address | Subnet Mask | VLAN |
|---|---|---|---|
| PC1 | 192.168.10.10 | 255.255.255.0 | 10 |
| PC2 | 192.168.10.20 | 255.255.255.0 | 10 |
| PC3 | 192.168.20.10 | 255.255.255.0 | 20 |
| PC4 | 192.168.20.20 | 255.255.255.0 | 20 |

For this basic lab, no default gateway is required because we are testing communication within individual VLANs.

---

## 8. Step 1 — Enter Switch Configuration Mode

Open the switch CLI.

```cisco
enable
configure terminal
```

---

## 9. Step 2 — Create VLANs

Create VLAN 10:

```cisco
vlan 10
name HR
exit
```

Create VLAN 20:

```cisco
vlan 20
name IT
exit
```

### Explanation

| Command | Purpose |
|---|---|
| vlan 10 | Creates or selects VLAN 10 |
| name HR | Assigns the name HR |
| exit | Returns to global configuration mode |

---

## 10. Step 3 — Assign Interfaces to VLAN 10

Connect PC1 and PC2 to FastEthernet 0/1 and 0/2.

```cisco
interface range fastEthernet 0/1 - 2
switchport mode access
switchport access vlan 10
exit
```

Both interfaces now belong to VLAN 10.

---

## 11. Step 4 — Assign Interfaces to VLAN 20

Connect PC3 and PC4 to FastEthernet 0/3 and 0/4.

```cisco
interface range fastEthernet 0/3 - 4
switchport mode access
switchport access vlan 20
exit
```

Both interfaces now belong to VLAN 20.

---

## 12. Step 5 — Verify VLAN Configuration

Run:

```cisco
end
show vlan brief
```

Expected relevant output:

```text
VLAN Name       Status    Ports
---- ---------- --------- -------------------
1    default    active    Fa0/5, Fa0/6, ...
10   HR         active    Fa0/1, Fa0/2
20   IT         active    Fa0/3, Fa0/4
```

The output confirms that the switch ports have been assigned to the correct VLANs.

---

## 13. Step 6 — Test Connectivity

Open the command prompt on PC1.

### Test 1: Same VLAN

```bash
ping 192.168.10.20
```

Expected result:

PC1 should communicate with PC2 because both devices belong to VLAN 10 and use addresses from the same subnet.

### Test 2: Different VLAN

Try communicating from PC1 to PC3.

```bash
ping 192.168.20.10
```

Expected result:

Communication should fail in this lab because no Layer 3 routing or default gateway has been configured.

**Important:** Communication between different VLANs requires Inter-VLAN Routing.

---

## 14. Useful Cisco Commands

| Command | Purpose |
|---|---|
| `show vlan brief` | Displays VLANs and assigned access ports |
| `show interfaces status` | Displays switch interface status |
| `show interfaces trunk` | Displays operational trunk interfaces |
| `show running-config` | Displays the current configuration |
| `show mac address-table` | Displays learned MAC addresses |
| `copy running-config startup-config` | Saves the configuration |

---

## 15. Common Troubleshooting Problems

If devices cannot communicate, check:

1. Are both PCs assigned valid IP addresses?
2. Are the subnet masks correct?
3. Are the devices assigned to the intended VLAN?
4. Are the cables connected properly?
5. Are the switch interfaces operational?
6. Is Inter-VLAN Routing required for the communication being tested?

Useful verification commands:

```cisco
show vlan brief
show interfaces status
show running-config
```

---

## 🔐 16. Cybersecurity Connection

VLANs are important for network security because they help organize and separate different groups of devices.

### Practical Applications

- Department-based segmentation
- Guest network separation
- IoT device isolation
- Limiting unnecessary broadcast traffic
- Supporting firewall and access-control policies

### Example

```text
Corporate Network
       |
       |--- VLAN 10 → Employees
       |
       |--- VLAN 20 → Guests
       |
       |--- VLAN 30 → IoT Devices
       |
       |--- VLAN 99 → Management
```

Traffic between these VLANs can be controlled through Layer 3 devices, firewalls, and access-control policies.

---

## 🧠 17. Key Takeaways

- VLAN stands for Virtual Local Area Network.
- VLANs logically divide a physical switched network.
- Each VLAN creates a separate Layer 2 broadcast domain.
- Access ports normally carry one data VLAN.
- Trunk ports can carry multiple VLANs.
- VLAN configuration can be verified using `show vlan brief`.
- Communication between different VLANs requires Layer 3 routing.

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- Cisco 2960 Switch
- IPv4
- Ping

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 16/60 — VLANs (Part 1) Completed ✅**

LEARN → PRACTICE → DOCUMENT → IMPROVE
