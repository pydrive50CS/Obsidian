---
title: "Linux Admin Recipe Book — Must-Do/Do-Not + Projects + First Useful Script"
tags: [linux, sysadmin, bash, automation, homelab, learning]
created: 2026-01-04
---

# Linux Admin Recipe Book (15+ years, distilled for a curious beginner)

> Read this like a checklist and a project roadmap. If you follow this note end-to-end, you’ll develop the instincts that separate “I can use Linux” from “I can run Linux systems.”

---

## 0) Your North Star (mindset that keeps you from becoming dangerous)

### Think in layers
- **Hardware → Kernel → init/systemd → filesystems → network → services → apps**
- When something breaks, you debug top-down *or* bottom-up, but you **always** ask: *Which layer is lying?*

### Your job is reliability
- Don’t be “clever”; be **predictable**.
- Automate anything you do twice.
- Write notes like you’ll hand the system to a stranger tomorrow.

---

## 1) Must-Do (core habits that pay off forever)

### A) Become fluent in the terminal fundamentals
Daily practice:
- Navigation: `pwd`, `ls -lah`, `cd`, `tree`
- Files: `cp -a`, `mv`, `rm -i`, `mkdir -p`, `ln -s`
- Searching: `grep -R`, `ripgrep (rg)`, `find`, `locate`
- Inspecting: `cat`, `less`, `head`, `tail -f`, `wc`, `sort`, `uniq`
- Text shaping: `cut`, `awk`, `sed`, `tr`, `xargs`, `jq` (for JSON)
- Processes: `ps aux`, `top/htop`, `pgrep`, `pkill`, `nice/renice`
- Disk: `df -h`, `du -sh`, `lsblk`, `mount`, `umount`
- Network: `ip a`, `ip r`, `ss -tulpn`, `ping`, `traceroute`, `dig`, `curl`
- Packages: `apt`, `dnf`, or `pacman` depending on distro

**Practice loop:**  
1. read `man <cmd>`  
2. run 3 examples  
3. write 1 note snippet  
4. repeat

---

### B) Learn how Linux boots and runs services (systemd basics)
You should be comfortable with:
- `systemctl status <service>`
- `systemctl start|stop|restart <service>`
- `journalctl -u <service> -f`
- `journalctl -p warning..alert --since "today"`
- `systemctl list-units --failed`

**Rule:** logs first, guesses last.

---

### C) Understand users, permissions, and ownership (this avoids 50% of beginner pain)
- Users/groups: `id`, `groups`, `/etc/passwd`, `/etc/group`
- Permissions: `chmod`, `chown`, `umask`
- Sudo: `sudo -l`, `/etc/sudoers` (edit with `visudo`)
- ACLs (later): `getfacl`, `setfacl`

**Fast mental model:**  
- Ownership decides *who* can try.  
- Permissions decide *what* they can do.  
- `sudo` decides *whether you’re allowed to become root*.

---

### D) Version-control your dotfiles and configs (even as a beginner)
Start a private git repo:
- `~/.bashrc`, `~/.zshrc`, `~/.vimrc` or `~/.config/nvim`
- `~/.ssh/config`
- Any service config you tweak in your lab

This builds the habit of reversible change.

**Link:** https://www.atlassian.com/git/tutorials (friendly Git guide)

---

### E) Create a “safe change” workflow (professional habit)
Every system change should be:
1. **Observed** (baseline: logs, metrics, state)
2. **Planned** (what, why, rollback)
3. **Applied** (smallest possible change)
4. **Verified** (does it work?)
5. **Documented** (what you did + why + how to undo)

---

### F) Backups are not optional
- Backups must be **automatic**
- Restore must be **tested**
- Keep at least:
  - 1 local copy
  - 1 separate-disk copy
  - (later) 1 remote/offsite copy

---

## 2) Must-Not (mistakes that create outages or data loss)

### A) Don’t live as root
- Use `sudo` for individual commands.
- If you *must* use a root shell: `sudo -i` and exit ASAP.

### B) Don’t run destructive commands without safety rails
Avoid:
- `rm -rf /something` without triple-check
- running random `curl ... | bash` from the internet
- `chmod -R 777` “to fix it”

Use safer patterns:
- `rm -i` or `trash-cli` (optional)
- confirm targets: `pwd`, `ls`, and `echo` the path first
- for scripts: use `set -euo pipefail` and sanity checks

### C) Don’t edit config files without a backup or diff
Before changing:
- `cp -a file file.bak.$(date +%F_%H%M%S)`
After changing:
- `diff -u file.bak... file`

### D) Don’t ignore logs (and don’t panic-restart everything)
- Logs tell the story.
- Restarting blindly destroys evidence.

### E) Don’t “learn” by permanently breaking your daily driver
Use a lab environment (containers/VMs) so you can be fearless **safely**.

---

## 3) Your First Week Learning Path (simple, high ROI)

### Day 1–2: CLI + filesystem + permissions
Goals:
- navigate, search, edit files
- understand permissions and ownership

### Day 3: processes + services + logs
Goals:
- find why something is failing using `systemctl` + `journalctl`

### Day 4: networking basics
Goals:
- diagnose DNS vs routing vs firewall issues

### Day 5: package management + updates
Goals:
- install/remove packages confidently; understand repositories

### Day 6–7: automate one pain-point with a script (see below)
Goal:
- ship one useful script and use it daily

---

## 4) Projects You Can Do on a Laptop (no new hardware)

### Project 1: “Mini Homelab” using containers (fast + safe)
Use either:
- **Docker** (widely used)
- **Podman** (daemonless; popular on Fedora/RHEL)

What to run:
- **Nginx** (web server)
- **Postgres** (database)
- **Prometheus + Grafana** (monitoring)
- **Uptime Kuma** (simple uptime monitor)
- **Gitea** (self-hosted Git; optional)

Learning outcomes:
- networking, ports, volumes, service health, logs, backups

**Links:**
- Docker Get Started: https://docs.docker.com/get-started/
- Podman: https://podman.io/getting-started/

---

### Project 2: Local DNS + ad-blocking (great networking practice)
Run **Pi-hole** in a container and point your laptop to it (even just for testing).
Learning outcomes:
- DNS, resolvers, systemd-resolved, troubleshooting connectivity

**Link:** https://pi-hole.net/

---

### Project 3: Self-hosted “status + logs” stack
- Loki + Promtail + Grafana (logs)
- Prometheus + node exporter (metrics)

Learning outcomes:
- logging pipelines, metrics, dashboards, alert thinking

**Links:**
- Grafana Loki: https://grafana.com/oss/loki/
- Prometheus: https://prometheus.io/docs/introduction/overview/

---

### Project 4: Build a “golden workstation” script
Write a script that:
- installs your tools
- configures shell + editor
- sets sane defaults
- creates directories you always use

Learning outcomes:
- reproducible systems, idempotency, package managers

---

### Project 5: SSH lab + hardening basics (safe security practice)
- generate SSH keys
- configure `~/.ssh/config`
- run an SSH server in a container/VM
- practice:
  - key auth
  - disabling password auth (lab only)
  - fail2ban (optional)

**Link:** https://www.ssh.com/academy/ssh

---

## 5) The #1 “Very Useful Script” You Should Write ASAP

### What it does
A **daily system snapshot + log bundle**:
- records disk, memory, CPU, top processes
- shows failed systemd units
- grabs recent warnings/errors from journal
- saves output in `~/sysreports/`
- optionally compresses older reports

Why it matters:
- it builds the habit of *observability*
- it gives you “before/after” evidence when troubleshooting
- it’s genuinely useful forever

---

## 6) Ready-to-Paste Script: `sysreport.sh`

> Paste this into `~/bin/sysreport.sh`, make executable, run daily or via a timer/cron.

```bash
#!/usr/bin/env bash
set -euo pipefail

# sysreport.sh — create a lightweight system health report + recent logs
# Works on most systemd Linux distros.

REPORT_DIR="${HOME}/sysreports"
HOST="$(hostname -s 2>/dev/null || hostname)"
TS="$(date +'%F_%H%M%S')"
OUT="${REPORT_DIR}/${HOST}_sysreport_${TS}.txt"

mkdir -p "${REPORT_DIR}"

{
  echo "=== SYSREPORT ==="
  echo "Host: ${HOST}"
  echo "Timestamp: $(date -Is)"
  echo "Uptime: $(uptime -p 2>/dev/null || uptime)"
  echo

  echo "=== KERNEL / OS ==="
  uname -a
  echo
  if command -v lsb_release >/dev/null 2>&1; then
    lsb_release -a 2>/dev/null || true
    echo
  fi

  echo "=== CPU / MEM (summary) ==="
  if command -v free >/dev/null 2>&1; then
    free -h
  fi
  echo
  if command -v vmstat >/dev/null 2>&1; then
    vmstat 1 5
  fi
  echo

  echo "=== DISK ==="
  df -hT
  echo
  echo "--- Top 20 largest directories in / (may take a bit) ---"
  # avoid crossing filesystem boundaries with -x if supported (GNU du)
  if du --help 2>/dev/null | grep -q -- "-x,"; then
    sudo -n true 2>/dev/null && SUDO="sudo" || SUDO=""
    $SUDO du -xhd1 / 2>/dev/null | sort -h | tail -n 20 || true
  else
    sudo -n true 2>/dev/null && SUDO="sudo" || SUDO=""
    $SUDO du -hd1 / 2>/dev/null | sort -h | tail -n 20 || true
  fi
  echo

  echo "=== NETWORK ==="
  ip a || true
  echo
  ip r || true
  echo
  echo "--- Listening sockets ---"
  ss -tulpn || true
  echo

  echo "=== TOP PROCESSES (CPU/MEM) ==="
  ps aux --sort=-%cpu | head -n 15
  echo
  ps aux --sort=-%mem | head -n 15
  echo

  echo "=== SYSTEMD HEALTH ==="
  if command -v systemctl >/dev/null 2>&1; then
    systemctl --failed || true
    echo
    echo "--- Last boot critical logs (warning..alert) ---"
    journalctl -b -p warning..alert --no-pager | tail -n 200 || true
    echo
    echo "--- Last 2 hours warnings/errors ---"
    journalctl --since "2 hours ago" -p warning..alert --no-pager | tail -n 200 || true
  else
    echo "systemctl not found (non-systemd system?)"
  fi
  echo

  echo "=== SMART QUICK LOOK (if available) ==="
  if command -v smartctl >/dev/null 2>&1; then
    echo "smartctl present. To inspect a disk: sudo smartctl -a /dev/<disk>"
  else
    echo "smartctl not installed (package: smartmontools)"
  fi
  echo

  echo "=== END ==="
} > "${OUT}"

echo "Report written to: ${OUT}"

# Compress reports older than 7 days to save space
if command -v find >/dev/null 2>&1 && command -v gzip >/dev/null 2>&1; then
  find "${REPORT_DIR}" -type f -name "*.txt" -mtime +7 -print0 2>/dev/null | while IFS= read -r -d '' f; do
    gzip -f "$f"
  done
fi
