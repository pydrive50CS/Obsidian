---
title: Linux Admin Apprenticeship (Arch) — In-Depth, Layman-Friendly Guide + Laptop Homelab + Starter Scripts
tags: [linux, arch, sysadmin, scripting, automation, homelab, obsidian]
created: 2026-01-04
---
---
title: Linux Admin Apprenticeship (Arch) — Complete In-Depth Guide + Must-Do/Don’t + Laptop Homelab + Starter Scripts
tags: [linux, arch, sysadmin, scripting, automation, homelab, obsidian]
---

# Linux Admin Apprenticeship (Arch) — Complete In-Depth Guide

This is a **single, self-contained Obsidian note** that compiles everything from our discussion into one place, with **layman-friendly explanations** plus **in-depth detail**. Links are included **only where useful**.

> **Philosophy:** Be curious, be safe, be repeatable.  
> Anything you do twice should become a script, a config, or a documented runbook.

---

## ## Table of Contents
- [[1) The Golden Rules]]
- [[2) The Linux Mental Model]]
- [[3)Arch Starter Kit- Your Learning Workspace|3) Arch Starter Kit- Your Learning Workspace]]
- [[4) Command Line Fluency]]
- [[5) Permissions & Ownership (Layman + Deep)]]
- [[6) systemd & Journald (Services + Logs)]]
- [[7) Networking Fundamentals (Practical)]]
- [[8) Package Management on Arch (Pacman + AUR Safety)]]
- [[9) Security Fundamentals (Beginner-Safe)]]
- [[10) Laptop-Only Homelab Projects]]
- [[11) The #1 Must-Have Script - Healthcheck + Triage Report]]
- [[12) Scripting Standards (So You Don’t Hurt Yourself)]]
- [[13) Troubleshooting Method (What Seniors Do)]]
- [[14) Weekly Routine (30 Minutes That Makes You Good)]]
- [[15) Break & Fix Lab Ideas]]
- [[16) Roadmap (Week-by-Week)]]](#16-roadmap-week-by-week)

---

# 1) The Golden Rules

## The 6 “Always”
1. **Always know what you’re changing**  
   Read the config. Understand defaults. Know the “blast radius”.
2. **Always be able to roll back**  
   Backups and version control (git) are non-negotiable.
3. **Always log your work**  
   Keep a sysadmin journal: a simple `docs/CHANGELOG.md`.
4. **Always test in a safe place first**  
   Use containers/VMs before touching your daily driver.
5. **Always make work reproducible**  
   Scripts + dotfiles + notes = your future safety.
6. **Always assume your future self is a stranger**  
   Write notes that make sense months later.

## The 6 “Never”
1. **Never run mystery commands as root**  
   Especially `curl ... | sh`, random one-liners, unknown AUR scripts.
2. **Never edit “important systems” without a backup and exit plan**
3. **Never disable security to “make it work”**  
   Fix the cause, don’t remove guardrails.
4. **Never change multiple things at once**  
   One change → test → commit.
5. **Never ignore logs**  
   “It doesn’t work” isn’t a symptom. Logs are the symptom.
6. **Never treat automation as optional**  
   Manual steps become future outages.

---

# 2) The Linux Mental Model

Linux becomes easy when you stop thinking in “apps” and start thinking in **building blocks**:

## A) Filesystem: “everything is a file”
Linux stores configuration and data as files:
- `/etc/` → configuration
- `/usr/bin/` → programs you run
- `/home/<you>/` → your personal files
- `/var/` → variable data (logs, caches, databases)
- `/tmp/` → temporary files

Key idea: if you can find and read files, you can admin most of Linux.

## B) Permissions: “who can do what”
Every file/dir has:
- an **owner** user,
- a **group**,
- permissions for **owner / group / others**.

Permissions:
- `r` read
- `w` write
- `x` execute (for directories: “can enter/access”)

## C) Processes: “running programs”
A process is a running program. Each process has:
- PID (process ID)
- user identity (UID)
- CPU/memory usage
- open files and network connections

## D) Services: “processes managed in the background”
On Arch the common manager is **systemd**.
Services:
- start at boot
- restart if they crash
- write logs
- are controlled with `systemctl`

## E) Networking: “how your machine talks”
You need:
- IP address (who you are)
- routes (how you reach outside networks)
- DNS (name → IP translation)
- ports (door numbers for services)

## F) Logs: “the system diary”
Logs are how Linux tells you:
- what it tried to do,
- what failed,
- and why.

---

# 3) Arch Starter Kit: Your Learning Workspace

## 3.1 Install essential tools
```bash
sudo pacman -Syu
sudo pacman -S --needed base-devel git ripgrep fd jq rsync wget curl tmux neovim less man-db man-pages
```
