In cybersecurity, finding an open port is like finding an unlocked door. **Service Enumeration** is the process of walking through that door to see exactly who lives there and what they are doing.

On a target like **Metasploitable**, this is where things get interesting because it is packed with old, vulnerable services.

---

# 🕵️‍♂️ Service Enumeration Guide

Topic: #Enumeration #Nmap #Metasploitable

Goal: Identify the exact name and version of every service running on the target.

---

## 1. The "Banner Grab" (Asking for an ID)

When you connect to a port, many services will "blurt out" what they are. This is called a **Banner**.

- **The Command:**
    
    Bash
    
    ```
    sudo nmap -sV [TARGET_IP]
    ```
    
- **Layman:** You walk up to a shop, and even before you ask, the owner says, "Welcome to Bob's Hardware Store, Version 2.1!"
    
- **Why it's crucial:** If you see `vsftpd 2.3.4`, you can immediately search the internet for "vsftpd 2.3.4 exploit" and find a known backdoor.
    

---

## 2. Using the "Scripting Engine" (NSE)

Nmap isn't just a scanner; it has a brain called the **Nmap Scripting Engine (NSE)**. It can run mini-programs to probe specific services for more info.

- **The Command (Default Scripts):**
    
    Bash
    
    ```
    sudo nmap -sC [TARGET_IP]
    ```
    
- **Layman:** Instead of just looking at the door, you send in a specialized building inspector. They check if the lock is loose, if the windows are thin, and if the alarm is actually plugged in.
    

---

## 3. The "Combo" (The Professional's Standard)

In a lab like your assignment, we usually combine Version Detection (`-sV`) and Default Scripts (`-sC`) for a complete picture.

- **The Command:**
    
    Bash
    
    ```
    sudo nmap -sS -sV -sC -p- [TARGET_IP]
    ```
    
    - `-sS`: Stealthy (SYN) scan.
        
    - `-sV`: Service Version detection.
        
    - `-sC`: Run default scripts.
        
    - `-p-`: Scan **all 65,535 ports** (Metasploitable hides things on high ports!).
        

---

## 📋 What to look for in Metasploitable results

When the scan finishes, look at the **SERVICE** and **VERSION** columns. In Metasploitable, you will likely see these "red flags":

|**Port**|**Service**|**Version**|**Why it's famous?**|
|---|---|---|---|
|**21**|ftp|vsftpd 2.3.4|Has a built-in backdoor (the ":)" exploit).|
|**80**|http|Apache httpd 2.2.8|Host to several vulnerable web apps like DVWA.|
|**139/445**|netbios/smb|Samba 3.x|Often vulnerable to "Username Map Script" exploits.|
|**1524**|ingreslock|-|A literal "backdoor" shell—connect and you're Root.|
|**3306**|mysql|MySQL 5.0.51a|Usually accessible with a blank root password.|

---

## 📁 Obsidian Note: Enumeration Workflow

> [!IMPORTANT] Your Lab Methodology
> 
> 1. **Initial Scan:** Find the target IP.
>     
> 2. **Full Port Scan:** `sudo nmap -p- [IP]` (Find every single open door).
>     
> 3. **Deep Enumeration:** `sudo nmap -sV -sC -p [PORT_LIST] [IP]` (Probe the specific doors you found).
>     
> 4. **Analysis:** Match those versions against **Searchsploit** or **Google**.
>     

---

**Now that you have the service versions, would you like me to show you how to use `searchsploit` in your Arch terminal to find the "keys" (exploits) for these specific versions?**