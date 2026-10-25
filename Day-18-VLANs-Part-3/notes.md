
# Day 18 — VLANs (Part 3) 🌐

## 📌 Topic: Inter-VLAN Routing

Today I learned how devices belonging to different VLANs can communicate using **Inter-VLAN Routing**.

I also explored Router-on-a-Stick configuration using Cisco Packet Tracer.

---

## 🎯 Learning Objectives

- Understand Inter-VLAN Routing
- Learn why VLANs require Layer 3 routing
- Understand Router-on-a-Stick
- Configure router subinterfaces
- Understand IEEE 802.1Q tagging
- Configure default gateways
- Verify connectivity between VLANs

---

## 1. What is Inter-VLAN Routing?

VLANs divide a physical network into separate logical broadcast domains.

By default, devices belonging to different VLANs cannot communicate directly through a Layer 2 switch.

Inter-VLAN Routing allows communication between different VLANs using a router or Layer 3 switch.

### Example

```text
VLAN 10 → HR Department
VLAN 20 → IT Department
```

If a device in VLAN 10 wants to communicate with a device in VLAN 20, its traffic must pass through a Layer 3 device.

---

## 2. What is Router-on-a-Stick?

Router-on-a-Stick is an Inter-VLAN Routing method in which a single physical router interface is divided into multiple logical subinterfaces.

Each subinterface is associated with a specific VLAN.

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
            HR          IT
```

The switch-to-router connection uses IEEE 802.1Q trunking.

---

# 💻 Practical Lab — Cisco Packet Tracer

## 3. Lab Requirements

Devices required:

- 1 Router supporting 802.1Q subinterfaces
- 1 Cisco 2960 Switch
- 2 PCs
- Copper straight-through cables

### Network Topology

```text
PC1 -------- SWITCH -------- PC2
              |
              |
            TRUNK
              |
            ROUTER
```

---

## 4. IP Addressing Table

| Device | Interface | IP Address | VLAN |
|---|---|---|---|
| PC1 | FastEthernet0 | 192.168.10.10/24 | 10 |
| PC2 | FastEthernet0 | 192.168.20.10/24 | 20 |
| Router | G0/0.10 | 192.168.10.1/24 | 10 |
| Router | G0/0.20 | 192.168.20.1/24 | 20 |

### Default Gateways

```text
PC1 → 192.168.10.1
PC2 → 192.168.20.1
```

---

## 5. Step 1 — Create VLANs on the Switch

Open the switch CLI:

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
- `name HR` assigns a name to VLAN 10.
- `vlan 20` creates VLAN 20.
- `name IT` assigns a name to VLAN 20.

---

## 6. Step 2 — Configure Access Ports

Assume:

- PC1 connects to Fa0/1.
- PC2 connects to Fa0/2.
- Router connects to Fa0/24.

Configure VLAN 10:

```cisco
interface fastEthernet 0/1
switchport mode access
switchport access vlan 10
exit
```

Configure VLAN 20:

```cisco
interface fastEthernet 0/2
switchport mode access
switchport access vlan 20
exit
```

---

## 7. Step 3 — Configure the Trunk Port

Configure the switch interface connected to the router:

```cisco
interface fastEthernet 0/24
switchport mode trunk
switchport trunk allowed vlan 10,20
no shutdown
exit
```

### Explanation

| Command | Purpose |
|---|---|
| `switchport mode trunk` | Configures the interface as a trunk |
| `switchport trunk allowed vlan 10,20` | Allows VLANs 10 and 20 on the trunk |
| `no shutdown` | Enables the interface |

---

## 8. Step 4 — Configure Router-on-a-Stick

Open the router CLI.

```cisco
enable
configure terminal
```

Enable the physical interface:

```cisco
interface gigabitEthernet 0/0
no shutdown
exit
```

The physical interface acts as the parent interface for the VLAN subinterfaces.

> Interface names can vary depending on the router model used in Packet Tracer.

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

### Important Commands Explained

| Command | Purpose |
|---|---|
| `interface g0/0.10` | Creates/selects a logical subinterface |
| `encapsulation dot1Q 10` | Associates the subinterface with VLAN 10 |
| `ip address` | Assigns the VLAN gateway address |
| `no shutdown` | Enables the physical interface |

---

## 9. Step 5 — Configure PC IP Addresses

### PC1 Configuration

Go to:

PC1 → Desktop → IP Configuration

```text
IP Address:      192.168.10.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

### PC2 Configuration

Go to:

PC2 → Desktop → IP Configuration

```text
IP Address:      192.168.20.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
```

---

## 10. Step 6 — Verify VLAN Configuration

On the switch:

```cisco
show vlan brief
```

Expected relevant entries:

```text
VLAN Name       Status    Ports
---- ---------- --------- --------
10   HR         active    Fa0/1
20   IT         active    Fa0/2
```

Verify the trunk:

```cisco
show interfaces trunk
```

Confirm that Fa0/24 is operating as a trunk and that VLANs 10 and 20 are allowed.

---

## 11. Step 7 — Verify Router Interfaces

On the router:

```cisco
show ip interface brief
```

Expected relevant output:

```text
Interface        IP-Address       Status  Protocol
Gi0/0            unassigned       up      up
Gi0/0.10         192.168.10.1     up      up
Gi0/0.20         192.168.20.1     up      up
```

Check the routing table:

```cisco
show ip route
```

The router should have connected routes for both VLAN networks.

---

## 12. Step 8 — Test Inter-VLAN Connectivity

Open PC1 → Desktop → Command Prompt.

First, test the default gateway:

```bash
ping 192.168.10.1
```

Next, test PC2:

```bash
ping 192.168.20.10
```

If all configurations are correct, PC1 should receive replies from PC2.

The first ping may occasionally time out while ARP resolution takes place.

---

## 13. How the Packet Travels

```text
PC1 (VLAN 10)
       |
       ↓
     SWITCH
       |
       ↓
  TRUNK CONNECTION
       |
       ↓
 ROUTER SUBINTERFACE
       |
       ↓
   LAYER 3 ROUTING
       |
       ↓
 ROUTER SUBINTERFACE
       |
       ↓
     SWITCH
       |
       ↓
PC2 (VLAN 20)
```

The router receives traffic from VLAN 10, performs Layer 3 forwarding, and sends it toward VLAN 20.

---

## 14. Troubleshooting Checklist

If Inter-VLAN Routing does not work, verify:

1. Correct VLAN creation.
2. Correct access-port assignment.
3. Correct trunk-port configuration.
4. Correct VLAN IDs on router subinterfaces.
5. Correct IP addresses and subnet masks.
6. Correct default gateways on PCs.
7. Physical router interface is enabled.
8. Router and switch interfaces are operational.

### Useful Commands

On the switch:

```cisco
show vlan brief
show interfaces trunk
show interfaces status
```

On the router:

```cisco
show ip interface brief
show ip route
show running-config
```

---

## 🔐 15. Cybersecurity Connection

Inter-VLAN Routing is important for designing organized enterprise networks.

It supports:

- Department-based network segmentation
- Traffic-flow management
- Firewall integration
- Access Control Lists (ACLs)
- Guest and internal network separation
- Secure network architecture

### Example

```text
VLAN 10 → Employees
VLAN 20 → Guests
VLAN 30 → Management
```

Inter-VLAN Routing enables communication between these networks.

However, routing alone does not restrict traffic. Security policies such as ACLs and firewall rules are needed to control which connections are permitted.

---

## 🧠 16. Key Takeaways

- VLANs create separate Layer 2 broadcast domains.
- Different VLANs require Layer 3 routing to communicate.
- Router-on-a-Stick uses one physical router interface with multiple subinterfaces.
- IEEE 802.1Q identifies VLAN traffic on trunk connections.
- Each VLAN requires an appropriate default gateway for communication outside its subnet.
- `show interfaces trunk` verifies switch trunking.
- `show ip route` verifies connected routes on the router.

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- Cisco Router
- Cisco 2960 Switch
- IPv4
- Ping

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 18/60 — VLANs (Part 3) Completed ✅**

LEARN → PRACTICE → DOCUMENT → IMPROVE
