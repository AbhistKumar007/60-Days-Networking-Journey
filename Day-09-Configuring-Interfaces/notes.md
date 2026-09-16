# Day 9 — Configuring Interfaces 🌐

## 📌 Topic

**Configuring Network Interfaces**

Today I practiced configuring a router interface using Cisco IOS commands in **Cisco Packet Tracer**.

---

## 🎯 Objectives

- Configure a router interface
- Assign an IPv4 address
- Enable an interface
- Verify interface status
- Troubleshoot basic interface issues

---

## 💻 Practical Lab — Cisco Packet Tracer

### 🖥️ Topology

```
PC1 -------- Switch -------- Router
```

---

### 🔧 Step 1 — Enter Privileged Mode

```
enable
```

### 🔧 Step 2 — Enter Configuration Mode

```
configure terminal
```

### 🔧 Step 3 — Select the Interface

```
interface gigabitEthernet 0/0
```

### 🔧 Step 4 — Assign IPv4 Address

```
ip address 192.168.1.1 255.255.255.0
```

### 🔧 Step 5 — Enable the Interface

```
no shutdown
```

The interface should change to an active state.

---

### 🔍 Step 6 — Verify the Interface

```
show ip interface brief
```

Example:

```
Interface              IP-Address      Status       Protocol
GigabitEthernet0/0     192.168.1.1     up           up
```

✅ `up/up` means:

- Interface is enabled
- Physical connection is working
- Line protocol is operational

---

### 🧪 Step 7 — Test Connectivity

From a connected PC:

```
ping 192.168.1.1
```

A successful reply confirms connectivity with the router interface.

---

## 🛠️ Useful Commands

**Check interface status**
```
show ip interface brief
```

**Check detailed interface information**
```
show interfaces gigabitEthernet 0/0
```

**Enter interface configuration**
```
interface gigabitEthernet 0/0
```

**Enable interface**
```
no shutdown
```

---

## ⚠️ Common Problems

If the interface is not working, check:

- IP address
- Subnet mask
- Cable connection
- Interface status
- `no shutdown`
- PC configuration

---

## 🔐 Cybersecurity Connection

Understanding interfaces is important for:

- Network segmentation
- Firewall configuration
- Secure network design
- Traffic monitoring
- Network troubleshooting

---

## 🧠 Key Takeaway

A router interface needs the correct IP address, subnet mask, and operational status to communicate with a network.

```
IP Address  →  192.168.1.1
Subnet Mask →  255.255.255.0
Status      →  up/up
```

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- IPv4

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 9/60 — Completed ✅**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
