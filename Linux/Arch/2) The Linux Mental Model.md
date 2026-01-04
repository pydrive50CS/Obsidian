
---
2) The Linux Mental Model

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
