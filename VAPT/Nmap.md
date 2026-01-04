
---

# 🛠️ Nmap Master Guide: Active Reconnaissance

Topic: #Nmap #ArchLinux #CyberSecurity #Metasploitable

Level: Expert Insights / Layman Explanations

---

## 🧭 The Core Concepts: Port States

Before running commands, you must understand what Nmap is telling you.

|**State**|**Layman Meaning**|**Technical Action**|
|---|---|---|
|**Open**|**Unlocked.** Someone is home and ready to talk.|The target sent a `SYN/ACK` packet.|
|**Closed**|**Locked.** The room exists, but it's empty.|The target sent an `RST` packet.|
|**Filtered**|**Security Guard.** You can't even get to the door.|A firewall dropped the packet (No response).|

---

## 🔍 Discovery: Finding Metasploitable

If you are in a lab and don't know the IP of the Metasploitable VM, follow these steps.

### 1. Identify Your Subnet

Run this in your Arch terminal:

Bash

```
ip addr show
```

_Look for `inet` under your ethernet or wifi (e.g., `192.168.1.15/24`). Your network range is `192.168.1.0/24`._

### 2. The Ping Sweep (Find All Live Hosts)

Bash

```
sudo nmap -sn 192.168.1.0/24
```

- **Layman:** Walking down the street and shouting "Is anyone home?" to every house.
    

### 3. The "Metasploitable" Fingerprint

Metasploitable is intentionally "leaky." It usually has Port 21 (FTP) and Port 23 (Telnet) open.

Bash

```
sudo nmap -p 21,23 --open 192.168.1.0/24
```

- **Why?** Normal computers rarely have Telnet (23) open. If you see an IP with both 21 and 23 open, **that is your target.**
    

---

## 🚀 Execution: Common Nmap Commands

Once you have the target IP (let's assume `192.168.1.50`), use these commands:

### A. The Stealth Scan (Default Choice)

Bash

```
sudo nmap -sS 192.168.1.50
```

- **Layman:** Touching the doorknob to see if it turns, but letting go before the alarm triggers.
    

### B. Service & Version Detection

Bash

```
sudo nmap -sV 192.168.1.50
```

- **Layman:** Peeking through the window to see exactly what brand of lock is on the door.
    

### C. OS Fingerprinting

Bash

```
sudo nmap -O 192.168.1.50
```

- **Layman:** Determining if the building is made of brick (Linux) or wood (Windows).
    

### D. The "Aggressive" Scan (Best for Labs)

Bash

```
sudo nmap -A 192.168.1.50
```

- **Note:** This does Versioning, OS detection, and Script scanning all at once. It is very "noisy."
    

---

## 📝 Professional Syntax Cheat Sheet

|**Flag**|**Purpose**|**Note**|
|---|---|---|
|`-sn`|Ping Scan|No port scanning, just "is it alive?"|
|`-p-`|Scan ALL ports|Scans 1-65535 instead of top 1000.|
|`-T4`|Timing Template|Speeds up the scan (T0 is slowest, T5 is fastest).|
|`-oN`|Save Output|Saves results to a readable text file.|
|`-sC`|Default Scripts|Runs safe scripts to find common vulnerabilities.|

---

## ⚠️ Expert Troubleshooting (Arch/VMs)

- **No Results?** If Metasploitable is in VirtualBox, ensure the Network Adapter is set to **Bridged** or **Host-Only**. If it is set to **NAT**, Nmap won't find it.
    
- **Permission Denied?** Arch requires `sudo` for Stealth (`-sS`) and OS (`-O`) scans because they require raw socket access.
    

---

**Would you like me to show you how to use Nmap's "Scripting Engine" (NSE) to check for specific vulnerabilities like EternalBlue on that IP?**

[[Service version enumeration]]