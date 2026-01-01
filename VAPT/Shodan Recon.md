---
type: CheatSheet
category: OSINT
tags: [shodan, recon, infrastructure, scanners]
updated: 2025-12-28
---

# 🛰️ Shodan Search Master Guide

Shodan identifies devices by scanning every possible IP and recording the "Banner"—the text response a service sends when you connect to it.

---

## 🛠️ Core Filter Syntax

| Filter | Purpose | Example |
| :--- | :--- | :--- |
| `port:` | Find specific open ports. | `port:3389` (RDP) |
| `product:` | Find specific software/vendor. | `product:"Apache httpd"` |
| `version:` | Find a specific software version. | `version:"2.4.49"` |
| `os:` | Filter by Operating System. | `os:"Windows 7"` |
| `org:` | Search by Organization (Owner). | `org:"Microsoft"` |
| `net:` | Search a specific IP range/CIDR. | `net:192.168.1.0/24` |
| `asn:` | Search by Autonomous System Number. | `asn:AS15169` (Google) |

---

## 🔍 Specific Use-Case Queries

### 1. Identify Open Ports & Services
> [!NOTE] Basic Service Discovery
> `port:21,22,23,445`
> *Lists all systems with FTP, SSH, Telnet, or SMB exposed.*

* **Find HTTP Proxies:** `port:8080 "200 OK"`
* **Find VNC Servers:** `port:5900 "authentication disabled"`
* **Find Databases:** `port:3306` (MySQL) or `port:27017` (MongoDB)

---

### 2. Identify Software Versions & Banners
To find specific versions (often for CVE hunting), combine `product` and `version`.

> [!DANGER] Vulnerability Hunting
> `product:"nginx" version:"1.14.0"`
> *Targeting systems running a specific, potentially outdated version.*

* **Banner Search:** `"Server: gws"` (Searching the exact text in a header)
* **SSH Banners:** `product:"OpenSSH" -port:22` (Finds SSH running on non-standard ports).



---

### 3. Identifying Exposed Systems
These queries find devices that are "publicly accessible" but shouldn't be.

* **Unsecured Webcams:** `has_screenshot:true "webcam"`
* **Industrial Control Systems (ICS):** `tag:ics` or `"in-tank inventory"` (Gas station monitors).
* **RDP (Remote Desktop):** `port:3389 has_screenshot:true` (Visual proof of an exposed login).
* **Printer Dashboards:** `product:"HP HTTP Server" "serial number"`

---

## 💻 CLI Power Commands
If you have the Shodan Python library installed (`pip install shodan`), use these for automation:

```bash
# Initialize with your API key
shodan init YOUR_API_KEY

# Get a summary of all ports and services for a domain
shodan domain pcampus.edu.np

# Download the first 100 results for a query to a file
shodan download results_file "product:apache" --limit 100

# Parse specific fields from a saved file
shodan parse --fields ip_str,port,org results_file.json.gz