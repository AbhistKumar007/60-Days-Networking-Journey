# Day 14 — Subnetting | Part 2 🔢🌐

## 📌 Topic

**Subnetting — Part 2**

Today I continued practicing subnetting with a focus on **block size, number of subnets, usable hosts, network addresses, and broadcast addresses**.

---

## 🎯 Objectives

- Calculate the number of subnets
- Calculate usable hosts
- Find the block size
- Identify network addresses
- Find first and last usable IP addresses
- Identify broadcast addresses
- Practice CIDR-based subnetting

---

## 🧠 Important Formulas

### Number of Subnets

```
Number of Subnets = 2^n

Where:
n = number of borrowed bits
```

### Total Addresses

```
Total Addresses = 2^h

Where:
h = number of host bits
```

### Usable Hosts

```
Usable Hosts = 2^h - 2
```

---

## 🔢 Example

Starting network:

```
192.168.1.0/24
```

We want to create /26 subnets.

```
Original Prefix = /24
New Prefix      = /26
```

**Borrowed Bits**

```
26 - 24 = 2 bits
```

Therefore:

```
Number of Subnets = 2² = 4
```

---

## 🧮 Host Calculation

For /26:

```
Host Bits = 32 - 26
          = 6
```

Total addresses:

```
2⁶ = 64
```

Usable hosts:

```
2⁶ - 2
= 64 - 2
= 62
```

Therefore, each /26 subnet has:

```
64 Total Addresses
62 Usable Host Addresses
```

---

## 📐 Subnet Mask

For /26:

```
CIDR        → /26
Subnet Mask → 255.255.255.192
```

Binary:

```
11111111.11111111.11111111.11000000
```

---

## 🔢 Block Size

Block size is calculated using:

```
256 - Subnet Mask Value
```

For /26:

```
256 - 192 = 64
```

Therefore, the subnet increments by:

```
0 → 64 → 128 → 192
```

---

## 📊 Subnet Table

For:

```
192.168.1.0/24 → /26
```

| Subnet | Network Address | First Usable   | Last Usable     | Broadcast       |
|--------|--------------------|------------------|--------------------|--------------------|
| 1      | 192.168.1.0        | 192.168.1.1      | 192.168.1.62       | 192.168.1.63       |
| 2      | 192.168.1.64        | 192.168.1.65      | 192.168.1.126      | 192.168.1.127      |
| 3      | 192.168.1.128       | 192.168.1.129     | 192.168.1.190      | 192.168.1.191      |
| 4      | 192.168.1.192       | 192.168.1.193     | 192.168.1.254      | 192.168.1.255      |

---

## 🧪 Practical Subnetting Process

When solving a subnetting problem, I follow these steps:

```
Step 1  → Identify the original network
Step 2  → Identify the required CIDR
Step 3  → Calculate borrowed bits
Step 4  → Calculate number of subnets
Step 5  → Calculate host bits
Step 6  → Calculate usable hosts
Step 7  → Find block size
Step 8  → Find network addresses
Step 9  → Find usable IP ranges
Step 10 → Find broadcast addresses
```

---

## 💻 Practice Example

### Question

Divide:

```
192.168.10.0/24
```

into /27 subnets.

### Solution

Borrowed bits:

```
27 - 24 = 3
```

Number of subnets:

```
2³ = 8
```

Host bits:

```
32 - 27 = 5
```

Total addresses:

```
2⁵ = 32
```

Usable hosts:

```
32 - 2 = 30
```

Subnet mask:

```
255.255.255.224
```

Block size:

```
256 - 224 = 32
```

Subnet increments:

```
0
32
64
96
128
160
192
224
```

---

## 🔐 Cybersecurity Connection

Subnetting is important for:

- Network segmentation
- Access control
- Firewall configuration
- Traffic isolation
- Network monitoring
- Secure network architecture

Dividing a network into smaller segments can help control how devices communicate with each other.

---

## 🧠 Key Takeaways

```
/26
↓
4 Subnets from a /24
↓
64 Addresses per Subnet
↓
62 Usable Hosts per Subnet
↓
Block Size = 64
```

And:

```
/27
↓
8 Subnets from a /24
↓
32 Addresses per Subnet
↓
30 Usable Hosts per Subnet
↓
Block Size = 32
```

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Subnetting Practice
- IPv4 Addressing

---

## 📈 Journey Progress

**60 Days Networking Journey**

**Day 14/60 — Subnetting Part 2 Completed ✅**

> LEARN → PRACTICE → DOCUMENT → IMPROVE
