# Day 15 — Subnetting (VLSM) | Lab 🔢🌐

## 📌 Topic

**VLSM — Variable Length Subnet Masking**

Today I practiced **VLSM (Variable Length Subnet Masking)** and learned how to divide a network into subnets of different sizes according to the number of hosts required.

---

## 🧠 What is VLSM?

VLSM allows us to use **different subnet masks within the same network**.

Instead of giving every subnet the same number of IP addresses, we allocate addresses according to the actual requirement.

```
Large Network
      ↓
     VLSM
      ↓
Different-sized Subnets
```

---

## 🎯 Objectives

- Understand VLSM
- Calculate required host addresses
- Select the appropriate subnet mask
- Allocate subnets efficiently
- Find network and broadcast addresses
- Calculate usable IP ranges
- Apply VLSM in Cisco Packet Tracer

---

## 🔄 FLSM vs VLSM

### FLSM — Fixed Length Subnet Masking

All subnets have the same size.

```
Network
 ├── /26
 ├── /26
 ├── /26
 └── /26
```

### VLSM — Variable Length Subnet Masking

Subnets can have different sizes.

```
Network
 ├── /25
 ├── /26
 ├── /27
 └── /28
```

---

## 💻 VLSM Lab

### 📋 Given Network

```
192.168.10.0/24
```

### Host Requirements

| Department     | Required Hosts |
|------------------|-------------------|
| Department A     | 100                |
| Department B     | 50                 |
| Department C     | 20                 |
| Department D     | 10                 |

---

## 🧮 Step 1 — Sort Requirements

In VLSM, allocate the largest requirement first.

```
100 hosts
50 hosts
20 hosts
10 hosts
```

---

## 🧮 Step 2 — Find Required Subnet Sizes

**Department A — 100 Hosts**

We need at least 100 usable addresses.

```
2^7 - 2 = 126
```

Therefore:

```
Subnet → /25
Mask   → 255.255.255.128
```

**Department B — 50 Hosts**

```
2^6 - 2 = 62
```

Therefore:

```
Subnet → /26
Mask   → 255.255.255.192
```

**Department C — 20 Hosts**

```
2^5 - 2 = 30
```

Therefore:

```
Subnet → /27
Mask   → 255.255.255.224
```

**Department D — 10 Hosts**

```
2^4 - 2 = 14
```

Therefore:

```
Subnet → /28
Mask   → 255.255.255.240
```

---

## 📊 Step 3 — VLSM Allocation

Starting network:

```
192.168.10.0/24
```

**Department A — 100 Hosts**

```
Network:     192.168.10.0/25
Mask:        255.255.255.128
First Host:  192.168.10.1
Last Host:   192.168.10.126
Broadcast:   192.168.10.127
Usable:      126 hosts
```

**Department B — 50 Hosts**

Next available address:

```
192.168.10.128
```

Allocation:

```
Network:     192.168.10.128/26
Mask:        255.255.255.192
First Host:  192.168.10.129
Last Host:   192.168.10.190
Broadcast:   192.168.10.191
Usable:      62 hosts
```

**Department C — 20 Hosts**

Next available address:

```
192.168.10.192
```

Allocation:

```
Network:     192.168.10.192/27
Mask:        255.255.255.224
First Host:  192.168.10.193
Last Host:   192.168.10.222
Broadcast:   192.168.10.223
Usable:      30 hosts
```

**Department D — 10 Hosts**

Next available address:

```
192.168.10.224
```

Allocation:

```
Network:     192.168.10.224/28
Mask:        255.255.255.240
First Host:  192.168.10.225
Last Host:   192.168.10.238
Broadcast:   192.168.10.239
Usable:      14 hosts
```

---

## 📋 Final VLSM Table

| Department | Hosts Required | Network           | CIDR | Usable Hosts | Broadcast          |
|--------------|-------------------|----------------------|--------|-----------------|------------------------|
| A            | 100                | 192.168.10.0         | /25    | 126              | 192.168.10.127          |
| B            | 50                 | 192.168.10.128        | /26    | 62               | 192.168.10.191          |
| C            | 20                 | 192.168.10.192        | /27    | 30               | 192.168.10.223          |
| D            | 10                 | 192.168.10.224        | /28    | 14               | 192.168.10.239          |

---

## 🔢 VLSM Calculation Pattern

For every subnet:

```
1. Identify required hosts
        ↓
2. Find required host bits
        ↓
3. Calculate subnet size
        ↓
4. Determine CIDR
        ↓
5. Find network address
        ↓
6. Find usable IP range
        ↓
7. Find broadcast address
```

---

## 💻 Practical — Cisco Packet Tracer

I used Cisco Packet Tracer to understand how VLSM can be applied to a real network.

The network can be divided into different departments based on their host requirements.

Example:

```
                 192.168.10.0/24
                        |
        ┌───────────────┼───────────────┐
        ↓               ↓               ↓
      Dept A          Dept B          Dept C
      /25              /26              /27
```

I practiced assigning IP addresses according to the VLSM plan.

---

## ⚠️ Common VLSM Mistakes

- Not allocating the largest subnet first
- Incorrect host calculation
- Overlapping subnet ranges
- Incorrect broadcast address
- Using the wrong subnet mask
- Forgetting the network address
- Forgetting the broadcast address

---

## 🔐 Cybersecurity Connection

VLSM is useful for designing organized networks with different requirements.

It can support:

- Network segmentation
- Department isolation
- Access control
- Firewall design
- Traffic management
- Efficient IP address allocation
- Secure network architecture

---

## 🧠 Key Takeaway

VLSM allows a single network to be divided into different-sized subnets based on actual host requirements.

```
192.168.10.0/24
       ↓
     VLSM
       ↓
 /25 → /26 → /27 → /28
```

The goal is to use IP addresses efficiently instead of assigning the same-size subnet to every network.

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- IPv4 Addressing
- Subnetting
- VLSM

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 15/60 — VLSM Lab Completed ✅**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
