In the world of active reconnaissance, **OS Fingerprinting** is the "DNA test" of the network. After you know which apps are running (Service Enumeration), fingerprinting tells you exactly what the underlying Operating System is (e.g., Linux Kernel 2.6.x, Windows Server 2008, etc.).

Here is the deep-dive guide for your Obsidian notes.

---

# 🧬 OS Fingerprinting: The DNA Test

**Topic:** #Fingerprinting #Nmap #ArchLinux #OSIdentification

## 🔍 What is OS Fingerprinting?

Every Operating System (OS) has a slightly different way of implementing the TCP/IP stack. When Nmap sends "weird" or malformed packets to a target, different OSs respond in unique ways.

- **The Technical Way:** Nmap looks at the **TTL** (Time to Live) values, **Window Size**, and how the target handles **TCP Options**. It compares these responses against a database of thousands of known signatures.
    
- **The Layman Way:** Imagine you can't see a person, but you hear them walk. A heavy person in boots (Windows) sounds different than a light person in sneakers (Linux). By listening to the "sound" of their network footsteps, you can identify them without seeing them.
    

---

## 🛠 How to Fingerprint with Nmap

Since this requires sending "raw" packets that don't follow the normal rules of networking, you **must** use `sudo` on your Arch machine.

### 1. Basic OS Detection

Bash

```
sudo nmap -O [TARGET_IP]
```

- **What it does:** Sends a series of TCP and UDP packets to open/closed ports and analyzes the reply.
    
- **The Result:** Nmap will tell you "Running: Linux 2.6.X" or "OS details: Microsoft Windows 7."
    

### 2. Aggressive Guessing

If Nmap is unsure (common with hardened systems or specific VMs), you can force it to try harder:

Bash

```
sudo nmap -O --osscan-guess [TARGET_IP]
```

- **Layman:** If the "footsteps" are muffled, you tell Nmap to make its best educated guess based on the closest match it has in its library.
    

---

## 🚦 Why Fingerprinting is Critical for Metasploitable

Metasploitable 2 usually runs on an old version of **Ubuntu Linux**.

Knowing the OS is the final piece of the puzzle before exploitation:

1. **Port 445 is open (SMB):** If the OS is **Windows**, I might use _EternalBlue_.
    
2. Port 445 is open (SMB): If the OS is Linux, I must use a Samba exploit.
    
    The service is the same, but the "lock" is different because the OS is different.
    

---

## 📁 The "Fingerprint" Checklist

> [!CHECKLIST] Identification Markers
> 
> - **TTL (Time to Live):** > * Usually **64** = Linux/Unix
>     
>     - Usually **128** = Windows
>         
>     - Usually **255** = Cisco/Network devices
>         
> - **Window Size:** Different OSs use different default buffer sizes for data.
>     
> - **TCP Sequence Numbers:** How "random" the computer generates its connection numbers.
>     

---

## 🚀 Final Summary of the "Recon Flow"

Now that you have completed the full cycle, your notes should show a clear path:

1. **Host Discovery:** Target is at `192.168.1.50`.
    
2. **Port Scanning:** Found ports 21, 22, 80, 445 are **OPEN**.
    
3. **Service Enumeration:** Port 21 is running `vsftpd 2.3.4`.
    
4. **OS Fingerprinting:** The machine is running **Linux (Ubuntu)**.
    

**You are now ready to look for vulnerabilities. Would you like me to explain how to use `searchsploit` to find the specific exploit for that `vsftpd` service you found?**

In the world of active reconnaissance, understanding the "path" to your target is just as important as knowing the target itself. To see the devices (routers, switches, firewalls) between you and your destination, we use a technique called **Path Discovery**.

On your Arch Linux machine, you have two primary ways to do this: the classic `traceroute` and the more powerful Nmap implementation.

---

# 🛣️ Path Discovery: Seeing Network Hops

Topic: #Networking #Traceroute #Nmap #Hops

Goal: Map the routers and devices between your Arch machine and the target.

---

## 🧭 What is a "Hop"?

Every time your data packet hits a router or a layer-3 switch on its way to the destination, that is called a **Hop**.

> [!QUOTE] Layman's Analogy
> 
> If sending an email is like sending a letter, a Hop is every post office the letter stops at along the way. Your local router is the first post office, your ISP is the second, and so on, until it reaches the recipient's mailbox.

---

## 🛠️ Method 1: The Nmap Traceroute (Professional)

Nmap has a built-in traceroute feature that is often more accurate than the standard system tool because it can use different types of packets (TCP/UDP/ICMP) to bypass firewalls.1

**The Command:**

Bash

```
sudo nmap --traceroute -sn [TARGET_IP]
```

- `-sn`: Skip the port scan (we just want the path).
    
- `--traceroute`: Traces the hop path.
    

Why use this?

If a standard ping is blocked by a firewall (Filtered), Nmap can try to "trace" using an open web port (like Port 80) to see the path anyway.

---

## 🛠️ Method 2: Standard Linux Traceroute

On Arch, you might need to install this first (`sudo pacman -S traceroute`), but it is the universal standard.

**The Command:**

Bash

```
traceroute [TARGET_IP]
```

**How to read the output:**

1. **Hop 1:** Usually your local gateway (e.g., `192.168.1.1`).
    
2. **Hop 2-5:** Your Internet Service Provider (ISP) infrastructure.
    
3. **Final Hop:** The target IP.
    
4. **Asterisks (`* * *`):** This means that specific router is "shy"—it's configured not to respond to discovery packets to stay hidden.
    

---

## 🛠️ Method 3: MTR (My Traceroute) - The Live View

If you want to see the network stability and hops in **real-time**, use `mtr`. It combines `ping` and `traceroute`.

**The Command:**

Bash

```
mtr [TARGET_IP]
```

- **Layman:** This is like watching a live GPS tracker on your packet as it moves through the internet. You can see which specific "post office" is slowing down your mail.2
    

---

## 📋 Obsidian Note: Hop Analysis for Labs

> [!IMPORTANT] Finding Metasploitable Hops
> 
> - **If Metasploitable is in the same network:** You will likely see only **1 hop**. This means there are no routers between you; you are on the same "floor" of the building.
>     
> - **If you see 0 hops:** This usually happens if you are scanning your own machine or if the network is "Flat."
>     
> - **Internal Devices:** If you see an unexpected hop (like `10.0.0.1`), you've discovered an internal router or a virtual gateway you didn't know existed.
>     

---

## 🚀 Summary Checklist

- [ ] **`ip addr`**: Confirm your starting point.
    
- [ ] **`nmap --traceroute`**: Map the path to the target.3
    
- [ ] **Check TTL**: Look at the "Time to Live" in your pings. If it's low (like 63 or 127 instead of 64 or 128), it confirms a router "subtracted" a hop from the packet.
    

**Would you like to see how to use Nmap to "Broadcast" scan and find every device on your local segment without knowing their IPs first?**