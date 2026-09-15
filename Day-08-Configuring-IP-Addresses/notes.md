# Day 8 — Configuring IP Addresses 🌐

## 📌 Topic
**Configuring IP Addresses**

Today I learned how to manually configure IPv4 addresses on network devices and verify connectivity using Cisco Packet Tracer.

---

## 🎯 Objectives

- Understand how to assign an IPv4 address
- Configure IP addresses on PCs
- Configure IP addresses on a router interface
- Verify IP configuration
- Test connectivity using `ping`

---

## 💻 Practical Lab — Cisco Packet Tracer

### 🖥️ Topology

```
PC1 -------- Switch -------- PC2
```

### 📋 IP Addressing

| Device | IP Address     | Subnet Mask     |
|--------|-----------------|-------------------|
| PC1    | 192.168.1.10    | 255.255.255.0     |
| PC2    | 192.168.1.20    | 255.255.255.0     |

Both PCs are in the same network:

```
Network: 192.168.1.0/24
```

---

### 🔧 Step 1 — Configure PC1

Go to:

```
PC1 → Desktop → IP Configuration
```

Enter:

```
IP Address:    192.168.1.10
Subnet Mask:   255.255.255.0
```

---

### 🔧 Step 2 — Configure PC2

Go to:

```
PC2 → Desktop → IP Configuration
```

Enter:

```
IP Address:    192.168.1.20
Subnet Mask:   255.255.255.0
```

---

### 🧪 Step 3 — Test Connectivity

Open:

```
PC1 → Desktop → Command Prompt
```

Run:

```
ping 192.168.1.20
```

If the configuration is correct, PC1 should receive replies from PC2.

Example:

```
Reply from 192.168.1.20
Reply from 192.168.1.20
Reply from 192.168.1.20
```

---

### 🔍 Step 4 — Verify IP Configuration

On the PC command prompt:

```
ipconfig
```

This displays the configured IP address and subnet mask.

---

## ⚠️ Common Troubleshooting

If ping fails, check:

- IP addresses
- Subnet masks
- Cable connections
- PC network interfaces
- Whether both devices are in the same subnet

---

## 🔐 Cybersecurity Connection

Understanding IP configuration is important for:

- Network monitoring
- Traffic analysis
- Firewall configuration
- Network troubleshooting
- Identifying devices on a network

---

## 🧠 Key Takeaway

An IP address allows a device to be identified and communicate on an IP network.

For this lab:

```
PC1 → 192.168.1.10
PC2 → 192.168.1.20
Network → 192.168.1.0/24
```

**Successful ping = Connectivity Verified ✅**

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Command Prompt
- IPv4

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 8/60 — Completed ✅**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
