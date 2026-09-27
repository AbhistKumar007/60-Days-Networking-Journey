# Day 13 — Subnetting | Part 1 🔢🌐

## 📌 Topic

**Subnetting — Part 1**

Today I started learning **Subnetting**, an important concept for designing and managing IP networks.

---

## 🧠 What is Subnetting?

Subnetting is the process of dividing a larger network into **smaller networks called subnets**.

```
Large Network
      ↓
  Subnetting
      ↓
Smaller Networks
```

Subnetting helps use IP addresses more efficiently and makes network management easier.

---

## 🎯 Objectives

- Understand subnetting
- Understand Network ID and Host ID
- Learn subnet masks
- Understand CIDR notation
- Calculate usable host addresses
- Divide a network into smaller subnets

---

## 📍 IPv4 Address

An IPv4 address contains 32 bits divided into four octets.

Example:

```
192.168.1.10
```

Binary representation:

```
11000000.10101000.00000001.00001010
```

Each octet contains:

```
8 bits
```

Total:

```
8 × 4 = 32 bits
```

---

## 🔢 Network ID and Host ID

An IP address can be divided into:

```
Network Portion + Host Portion
```

Example:

```
192.168.1.10/24
```

With /24:

```
Network Bits → 24
Host Bits    → 8
```

So:

```
Network → 192.168.1.0
Host    → 10
```

---

## 📋 Subnet Mask

For a /24 network:

```
CIDR:       /24
Subnet Mask: 255.255.255.0
```

Binary:

```
11111111.11111111.11111111.00000000
```

---

## 🧮 Host Calculation

Formula:

```
Usable Hosts = 2^h - 2
```

Where:

```
h = number of host bits
```

For /24:

```
Host bits = 32 - 24
          = 8
```

Therefore:

```
2^8 - 2
= 256 - 2
= 254 usable hosts
```

---

## 🔢 Example — /24 Network

Network:

```
192.168.1.0/24
```

| Information        | Value            |
|----------------------|-------------------|
| Network Address      | 192.168.1.0       |
| First Usable IP      | 192.168.1.1       |
| Last Usable IP       | 192.168.1.254     |
| Broadcast Address    | 192.168.1.255     |
| Usable Hosts         | 254               |
| Subnet Mask          | 255.255.255.0     |

---

## ✂️ Dividing a /24 Network into /26

Original network:

```
192.168.1.0/24
```

New prefix:

```
/26
```

Subnet mask:

```
255.255.255.192
```

This creates 4 subnets:

```
192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26
```

Each subnet contains:

```
64 total addresses
62 usable host addresses
```

---

## 📊 /26 Subnet Table

| Subnet | Network Address | Usable Range   | Broadcast      |
|--------|-------------------|------------------|------------------|
| 1      | 192.168.1.0       | .1 – .62         | .63              |
| 2      | 192.168.1.64      | .65 – .126       | .127             |
| 3      | 192.168.1.128     | .129 – .190      | .191             |
| 4      | 192.168.1.192     | .193 – .254      | .255             |

---

## 💻 Practical Practice

I practiced subnetting by dividing:

```
192.168.1.0/24
```

into smaller /26 networks.

I calculated:

- Network addresses
- First usable addresses
- Last usable addresses
- Broadcast addresses
- Number of usable hosts

---

## 🔐 Cybersecurity Connection

Subnetting is useful for:

- Network segmentation
- Access control
- Firewall design
- Isolating systems
- Reducing unnecessary network exposure
- Managing network traffic

---

## 🧠 Key Takeaways

```
IPv4 = 32 bits

/24
↓
24 Network Bits
8 Host Bits
↓
254 Usable Hosts
```

For /26:

```
/24 → /26
     ↓
4 Subnets
     ↓
62 Usable Hosts per Subnet
```

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- IPv4 Calculator
- Subnetting Practice

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 13/60 — Subnetting Part 1 Completed ✅**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
