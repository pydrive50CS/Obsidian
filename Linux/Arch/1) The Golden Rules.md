---
title: Linux Admin Apprenticeship (Arch) — Complete In-Depth Guide + Must-Do/Don’t + Laptop Homelab + Starter Scripts
tags: [linux, arch, sysadmin, scripting, automation, homelab, obsidian]
---
# 1) The Golden Rules (Read This Before You Touch Anything)

> **If you remember nothing else from your apprenticeship, remember this chapter.**  
> These rules come from real outages, real data loss, and real “why did I do that?” moments.

As a Linux administrator, your job is not “knowing commands.”  
Your job is keeping systems **predictable, recoverable, and boring**.

---

## 1.1 Rule: Never Experiment on What You Can’t Lose

### Layman Explanation
If breaking something will ruin your day, **don’t experiment on it**.

### Sysadmin Reality
Learning requires failure — but failure must be **controlled**.

### MUST DO (Practice This)
- Use your laptop as a **lab**
- Use **separate users**, **VMs**, **containers**, or **chroots** for experiments
- Assume **every command** can destroy data if used wrong

### MUST NOT DO
- Copy random commands from the internet and run them as root
- “Just try it” on your main system without a recovery plan
- Skip backups because “it probably won’t break”

### “Safe Experiment” Pattern
Before changing anything, do **this exact flow**:

```bash
# 1) Inspect first
ls -lah

# 2) Confirm what you are about to touch
pwd
whoami

# 3) Dry-run / preview whenever possible
# (examples)
pacman -Si <pkgname>
systemctl status <service>

# 4) Make a backup copy before editing files
sudo cp -av /etc/someconfig.conf /etc/someconfig.conf.bak
```

## 1.2 Rule: Read Before You Press Enter

### Layman Explanation

Linux does **exactly what you ask** — even if what you ask is a disaster.

### The 3-Second Pre-Enter Check

Ask yourself:

1. What files will this touch?
    
2. Which user is running it?
    
3. Can I undo it?
    

### MUST DO

- Use `ls` before `rm`
    
- Prefer interactive confirmations for deletes
    
- Use `less` to read configs/logs before editing
    

```bash
# Inspect before you change
ls -lah
tree -L 2 2>/dev/null || true

# Read files safely (read-only)
less /etc/pacman.conf
less /etc/fstab

# Safer deletes
rm -ri somefolder/

```

### MUST NOT DO

- Run commands you don’t understand
    
- Use `rm -rf` casually
    
- Use wildcards `*` unless you have previewed what they match
```bash
# Preview what * will expand to
echo /tmp/*
```

---

## 1.3 Rule: Root Is a Loaded Gun

### Layman Explanation

`root` is “god mode.” God mode has **no undo button**.

### Sysadmin Reality

Root bypasses permissions and safety rails.  
One typo can wipe the system.

### MUST DO

- Use `sudo` for single commands
    
- Keep root sessions short
    
- Log what you do

```bash
# Confirm identity
whoami
id

# Use sudo only when needed
sudo -v  # refresh sudo auth timestamp
sudo ls -lah /root
```

### MUST NOT DO

- Daily drive as root
    
- Run unknown scripts with sudo
    
- Stay logged into a root shell for long periods
    

```bash
# BAD: running internet paste as root
# curl ... | sudo bash
```

> **Mental model:** Root is for surgery, not daily life.

---

## 1.4 Rule: Everything Is a File (This Is Your Superpower)

### Layman Explanation

Linux treats settings, devices, and system info like files.

### Why This Matters

If everything is a file, you can:

- read it
    
- back it up
    
- version-control it
    
- restore it
    

### Quick Proofs (Try These)

```bash
# Hardware and kernel info as "files"
cat /proc/cpuinfo | less
cat /proc/meminfo | less

# Devices are represented as files
ls -lah /dev | less

# System settings are in /etc
ls -lah /etc | less
```

---

## 1.5 Rule: Logs Are Where Truth Lives

### Layman Explanation

When something breaks, Linux usually tells you why — in logs.

### Sysadmin Reality

Guessing is amateur mode. Logs are professional mode.

### MUST DO

- Use `journalctl` early
    
- Always copy exact errors
    
- Check service status + logs together
    

```bash
# General system log exploration
journalctl -xe

# Logs for a specific service
journalctl -u sshd --no-pager
journalctl -u NetworkManager --since "30 minutes ago" --no-pager

# Follow logs live (like tail -f)
journalctl -f
```

### MUST NOT DO

- Reboot to “fix it” without checking logs (you lose evidence)
    
- Change random configs without understanding the error
    

> **Rule:** Observe → Measure → Change one thing → Observe again.

---

## 1.6 Rule: Automate Repetition, Not Ignorance

### Layman Explanation

Scripts should encode understanding — not hide confusion.

### Sysadmin Reality

Automation amplifies mistakes. If you are wrong manually, you’ll be wrong faster in a script.

### The Correct Learning Order

1. Do it manually
    
2. Understand every step
    
3. Break it
    
4. Fix it
    
5. Script it
    

### Example: Bad vs Good “Automation”

❌ Bad (dangerous and pointless)

`rm -rf /var/log/*`

✅ Good (collect info safely first)

`du -sh /var/log/* 2>/dev/null | sort -h`

---

## 1.7 Rule: Backups Are Not Optional

### Layman Explanation

If data exists in one place, it’s already lost — you just don’t know it yet.

### Sysadmin Reality

Hardware dies. Humans make mistakes. Updates break things.

### Minimum Standard: 3-2-1 (Simple Version)

- 3 copies of important data
    
- 2 different storage types/locations
    
- 1 copy offline or snapshot-based
    

### Laptop-Only Backup Starter (No New Hardware Required)

You can back up to:

- another partition
    
- a folder on the same disk (still better than nothing)
    
- a second user-owned location
    
- a VM disk image
    
- cloud (optional)
    

#### Simple “home backup” to a folder (starter step)
```bash
mkdir -p ~/BACKUP_HOME
rsync -av --delete \
  --exclude ".cache/" \
  --exclude "Downloads/" \
  --exclude ".local/share/Trash/" \
  "$HOME/" "$HOME/BACKUP_HOME/"
```

> Note: Same-disk backups don’t protect from disk failure — but they DO protect from “oops I deleted it.”

### Snapshot Option (Best on Btrfs)

If you use Btrfs on Arch, snapshots are a powerful safety net.  
(We’ll cover this deeply in later topics, but the golden rule is: **snapshots before risky changes**.)

---

## 1.8 Rule: Documentation Is Part of the Job

### Layman Explanation

Your future self is a stranger. Leave notes.

### Sysadmin Reality

You will forget why you changed something. Notes prevent repeated pain.

### Minimum “Change Note” Template
```bash
## Change
## Reason
## Commands
## Files Changed
## How to Roll Back
## What I Learned
```

### What to Document Every Time
```bash
# Always capture:
date
uname -a
whoami
pacman -Q | wc -l
```

---

## 1.9 Rule: Break Things Intentionally (In a Safe Lab)

### Layman Explanation

You don’t learn recovery by avoiding failure.

### Sysadmin Reality

Seniors don’t fear outages because they’ve rehearsed recovery.

### Safe Breaking Ideas (Laptop Only)

- Stop a service and bring it back
    
- Break DNS and fix it
    
- Fill disk space and recover
    
- Misconfigure a config file and restore from backup
    
```bash
# Example: stop and start a service safely
systemctl status sshd --no-pager 2>/dev/null || true
sudo systemctl stop sshd 2>/dev/null || true
sudo systemctl start sshd 2>/dev/null || true
sudo systemctl status sshd --no-pager 2>/dev/null || true
```
---

## 1.10 Rule: Humility Beats Ego

### Layman Explanation

Linux doesn’t care how smart you feel.

### Sysadmin Reality

Arrogance creates outages. Calm curiosity prevents them.

### The Professional Sentence

`"I don't know yet — but I can reproduce it, observe it, and find the cause."`

---

## 1.11 The “Senior Admin” Troubleshooting Mindset (Mini Preview)

When something breaks, seniors usually do this:
```bash
# 1) What changed recently?
# (check pacman logs, system logs, configs)
sudo less /var/log/pacman.log

# 2) What is failing right now?
systemctl --failed
journalctl -xe

# 3) Reduce to a simple test
ping -c 2 1.1.1.1
ping -c 2 archlinux.org
```
> **Do one change at a time.**  
> If you change 5 things, you will never know which one fixed it — or caused it.

---

## 1.12 Quick “Must-Do” Checklist (Do This Today)

This is your minimum foundation **before** you go deeper:
```bash
# Update system (Arch basics)
sudo pacman -Syu

# Install essential tools you'll use daily
sudo pacman -S --needed \
  base-devel \
  git \
  curl \
  wget \
  rsync \
  openssh \
  man-db \
  man-pages \
  less \
  tree

# Confirm you can read manuals
man pacman
man systemctl
man journalctl
```

---

## 1.13 Quick “Must-Not-Do” Checklist (Avoid These Always)

```bash
- Don’t run copy-pasted commands as root without understanding.
- Don’t use rm -rf unless you have inspected the target.
- Don’t reboot before collecting logs (you lose evidence).
- Don’t change many things at once when debugging.
- Don’t keep secrets in plain text configs.
- Don’t skip backups before major changes.
```

---