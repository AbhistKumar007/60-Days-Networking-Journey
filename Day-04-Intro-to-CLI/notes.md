# 🌐 Day 04 - Introduction to Cisco CLI

## 📌 Overview

The **Command Line Interface (CLI)** is a text-based interface used to configure and manage network devices.

Instead of using a graphical interface, network administrators can use commands to:

- Configure devices
- Monitor network status
- Troubleshoot problems
- View configurations
- Configure interfaces
- Verify connectivity

Cisco networking devices commonly use **Cisco IOS (Internetwork Operating System)**.

---

# 🎯 Learning Objectives

In this topic, I learned:

- What CLI is
- What Cisco IOS is
- Different Cisco CLI modes
- How to navigate between CLI modes
- Basic Cisco commands
- How to configure a hostname
- How to view device information
- How to view configurations
- How to check interfaces
- How to save configurations
- Basic CLI troubleshooting

---

# 🔹 1. What is Cisco IOS?

**Cisco IOS** stands for **Cisco Internetwork Operating System**.
It is the operating system used on many Cisco networking devices.

Cisco IOS provides commands for:
- Device configuration
- Routing
- Switching
- Security
- Monitoring
- Troubleshooting

---

# 🏗️ Cisco CLI Modes

Cisco IOS has different command modes. The most important modes are:

`User EXEC Mode` -> `Privileged EXEC Mode` -> `Global Configuration Mode` -> `Specific Configuration Modes`

### 🔹 1. User EXEC Mode
User EXEC mode is the initial mode after logging into a Cisco device.
This mode provides limited access to commands.
- **Prompt:** `Switch>`
- **Example:** `Switch> show version`

### 🔹 2. Privileged EXEC Mode
Privileged EXEC mode provides access to more advanced commands.
- **Enter it using:** `Switch> enable`
- **Prompt:** `Switch#`
- **Example:** `Switch# show running-config`

### 🔹 3. Global Configuration Mode
Global Configuration Mode is used to configure the device.
- **Enter it using:** `Switch# configure terminal` (or `conf t`)
- **Prompt:** `Switch(config)#`

### 🔹 4. Interface Configuration Mode
This mode is used to configure a specific interface.
- **Example:** `Switch(config)# interface fastethernet 0/1`
- **Prompt:** `Switch(config-if)#`
*(Commands entered here affect that specific interface.)*

---

# 🔄 Moving Between CLI Modes

    Switch>
       |
       | enable
       ↓
    Switch#
       |
       | configure terminal
       ↓
    Switch(config)#
       |
       | interface fa0/1
       ↓
    Switch(config-if)#

### ⬅️ Going Back

To exit the current configuration mode:
`Switch(config-if)# exit`

Example:
`Switch(config-if)# exit`
`Switch(config)#`

To return directly to Privileged EXEC mode:
`end` or `Ctrl + Z`

Example:
`Switch(config-if)# end`
`Switch#`

---

# 🛠️ Basic Cisco Commands

**1. `enable`**
Moves from User EXEC mode to Privileged EXEC mode.
`Switch> enable`

**2. `configure terminal`**
Enters Global Configuration Mode.
`Switch# configure terminal`

**3. `hostname`**
Changes the device hostname.
`Switch(config)# hostname SW1`
The prompt changes to: `SW1(config)#`

**4. `show running-config`**
Displays the current active configuration.
`SW1# show running-config`
*(The running configuration is stored in RAM).*

**5. `show startup-config`**
Displays the configuration saved in NVRAM.
`SW1# show startup-config`

---

# 💾 Saving Configuration

### Running Configuration vs Startup Configuration

| Configuration | Location | Purpose |
| :--- | :--- | :--- |
| **Running-config** | RAM | Current active configuration |
| **Startup-config** | NVRAM | Configuration used after reboot |

**Important:** If changes are made to the running configuration but are not saved, they may be lost after a reboot.

To save the current configuration:
`SW1# copy running-config startup-config`

Another commonly used command:
`SW1# write memory` (or `wr`)

---

# 🔍 Useful Show Commands

Cisco devices provide many `show` commands for monitoring and troubleshooting.

### Show Version
`show version`
Displays information such as: IOS version, device model, uptime, memory, configuration register.

### Show IP Interface Brief
`show ip interface brief`
This is one of the most useful commands for checking interface status.
Example:
    Interface              IP-Address      Status      Protocol
    FastEthernet0/1        unassigned      up          up
    FastEthernet0/2        unassigned      down        down

### Show Interfaces
`show interfaces`
Provides detailed information about interfaces. It can show: Interface status, Errors, Traffic statistics, MAC address, Speed, Duplex, Packets.

---

# ⚙️ Device Configuration

### 🔐 Basic Password Configuration
A privileged EXEC password can be configured using:
`SW1(config)# enable secret MyPassword`
*(In real environments, use strong passwords and follow organizational security policies.)*

### 🌐 Configuring a Switch Management IP
A Layer 2 switch can be assigned an IP address to allow management.
`SW1(config)# interface vlan 1`
`SW1(config-if)# ip address 192.168.1.2 255.255.255.0`
`SW1(config-if)# no shutdown`
*(The IP address is assigned to the switch virtual interface - SVI).*

### 🔌 no shutdown
Some interfaces can be administratively disabled. To enable an interface:
`SW1(config-if)# no shutdown` (Abbreviation: `no shut`)

---

# 🧭 CLI Command Navigation

Cisco IOS provides useful shortcuts.
- **`?`** : Displays available commands (e.g., `SW1# ?`).
- **`Tab`** : Automatically completes a command (e.g., `conf` + TAB becomes `configure`).
- **`Up Arrow`** : Displays previously entered commands.
- **`Ctrl + Z`** : Returns directly to Privileged EXEC mode.

---

# 📊 Interface Status

- **Up / Up**: Usually means the interface and line protocol are operational.
- **Administratively Down**: The interface has been manually disabled. Use `no shutdown` when appropriate.
- **Down / Down**: This may indicate a physical or connectivity issue (Cable problem, Device disconnected, Incorrect interface, Remote-side issue).

---

# 🧪 Hands-On Lab — Cisco Packet Tracer

**Lab Objective:** Practice basic Cisco CLI commands using a switch in Cisco Packet Tracer.

### Step 1 — Add a Switch
Open Cisco Packet Tracer. Add `1 × Cisco Switch`.

### Step 2 — Open CLI
Click the switch. Go to `CLI`. You should see `Switch>`.

### Step 3 — Enter Privileged Mode
`Switch> enable`

### Step 4 — Enter Configuration Mode
`Switch# configure terminal`

### Step 5 — Change Hostname
`Switch(config)# hostname SW1`

### Step 6 — Check Configuration
`SW1(config)# end`
`SW1# show running-config`

### Step 7 — Check Interfaces
`SW1# show ip interface brief`

### Step 8 — Save Configuration
`SW1# copy running-config startup-config` (Press Enter when prompted).

---

# 🔐 Why CLI is Important for Cybersecurity

CLI knowledge is important for cybersecurity because many security and networking tools are command-line based.
Examples: Cisco IOS, Kali Linux, Nmap, Wireshark/tshark, tcpdump, Scapy, Linux networking commands.

Cisco CLI can be used to:
- Inspect network configurations
- Check interfaces
- Troubleshoot connectivity
- Configure access controls
- Monitor network behavior
- Configure VLANs and routing
- Apply security policies

---

# 🧠 Key Takeaways

- CLI allows network devices to be managed using commands.
- Cisco IOS is the operating system used on many Cisco devices.
- User EXEC mode starts with `>`.
- Privileged EXEC mode uses `#`.
- Global Configuration Mode uses `(config)#`.
- Interface Configuration Mode uses `(config-if)#`.
- `show` commands are important for monitoring and troubleshooting.
- Running-config is stored in RAM.
- Startup-config is stored in NVRAM.
- Configuration should be saved when changes need to survive a reboot.
- CLI skills are essential for networking and cybersecurity.

---

# 📚 Commands Cheat Sheet

| Command | Purpose |
| :--- | :--- |
| `enable` | Enter Privileged EXEC mode |
| `configure terminal` | Enter Global Configuration Mode |
| `hostname SW1` | Change hostname |
| `show running-config` | View active configuration |
| `show startup-config` | View saved configuration |
| `show version` | View device/IOS information |
| `show ip interface brief` | Check interface status |
| `show interfaces` | View detailed interface information |
| `copy running-config startup-config` | Save configuration |
| `exit` | Exit current mode |
| `end` | Return to Privileged EXEC mode |
| `no shutdown` | Enable an administratively disabled interface |
| `?` | Show available commands |

### 🚀 Conclusion
Today I learned how to interact with Cisco networking devices using the Command Line Interface. CLI is an essential skill for networking, security, and cloud.

> LEARN → PRACTICE → DOCUMENT → IMPROVE
