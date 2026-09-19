# Day 11 — Configuring Static Routes 🛣️🌐

## 📌 Topic

**Configuring Static Routes — Lab 1**

Today I practiced configuring **static routes** between different networks using Cisco Packet Tracer.

---

## 🎯 Objectives

- Understand static routing
- Configure routes manually
- Understand next-hop addresses
- Verify routing tables
- Test connectivity between different networks

---

## 🧠 What is Static Routing?

Static routing is a method where an administrator manually tells a router how to reach a specific network.

The route is configured manually using the:

```
Destination Network
+
Subnet Mask
+
Next-Hop IP Address
```

---

## 💻 Practical Lab — Cisco Packet Tracer

### 🖥️ Basic Topology

```
PC1 ── Switch ── R1 ───── R2 ── Switch ── PC2
```

Example networks:

```
LAN 1 → 192.168.1.0/24
LAN 2 → 192.168.2.0/24
```

---

### 🔧 Step 1 — Configure Static Route

On R1, use:

```
enable
configure terminal
ip route 192.168.2.0 255.255.255.0 <R2-next-hop-IP>
```

On R2, configure the return route:

```
enable
configure terminal
ip route 192.168.1.0 255.255.255.0 <R1-next-hop-IP>
```

Replace the next-hop IP with the actual IP address of the neighboring router interface.

---

### 🔍 Step 2 — Verify Routing Table

Use:

```
show ip route
```

A static route is displayed with:

```
S
```

Example:

```
S    192.168.2.0/24 [1/0] via <next-hop-IP>
```

---

### 🧪 Step 3 — Test Connectivity

From PC1:

```
ping 192.168.2.x
```

If the routing configuration is correct, PC1 should be able to communicate with PC2.

---

## 🔎 Useful Commands

**View routing table**
```
show ip route
```

**Test connectivity**
```
ping <destination-IP>
```

**Trace the path**
```
traceroute <destination-IP>
```

**Check interface status**
```
show ip interface brief
```

---

## ⚠️ Troubleshooting Checklist

If the ping fails, check:

- IP addresses
- Subnet masks
- Router interfaces
- Interface status
- `no shutdown`
- Static route configuration
- Return route
- Cable connections

---

## 🔐 Cybersecurity Connection

Routing knowledge is useful for:

- Network segmentation
- Traffic-flow analysis
- Firewall configuration
- Secure network design
- Network troubleshooting

Understanding how traffic moves between networks is an important foundation for network security.

---

## 🧠 Key Takeaway

```
Static Route
     ↓
Destination Network
     ↓
Subnet Mask
     ↓
Next-Hop IP
     ↓
Packet Forwarding
```

Static routes provide a simple way to manually control how traffic reaches remote networks.

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- Ping
- Traceroute

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 11/60 — Lab 1 Completed ✅**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
