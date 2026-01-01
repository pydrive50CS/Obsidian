---
type: CheatSheet
category: OSINT
tags: [dns, lookup, reconnaissance, networking, security]
updated: 2025-12-28
---

# 🌐 DNS Lookup & Enumeration Master Guide

The Domain Name System (DNS) is the "Phonebook of the Internet." For a security expert, DNS records are a map of an organization's internal and external infrastructure.

## Builtwith.com
https://builtwith.com This is for personnel information

---

## 📋 Common DNS Record Types
Understanding what each record reveals is critical for OSINT.

| Record | Name | OSINT Value |
| :--- | :--- | :--- |
| **A** | Address | Maps domain to **IPv4**. Reveals the web server's location. |
| **AAAA** | Quad-A | Maps domain to **IPv6**. Essential for modern infra. |
| **CNAME** | Canonical Name | Aliases. Often reveals third-party services (e.g., `shop.target.com` -> `shopify.com`). |
| **MX** | Mail Exchange | Identifies mail servers. Helps find the email provider (e.g., Google, Outlook). |
| **TXT** | Text Record | Often contains **SPF/DMARC** (security policies) or verification codes (Google, AWS). |
| **NS** | Name Server | Shows who manages the DNS. (e.g., Cloudflare, Route53). |
| **SOA** | Start of Authority | Administrative data. May reveal the admin's email address. |
| **PTR** | Pointer | Used for **Reverse DNS** (IP to Domain). |

---

## 🛠️ Top Online DNS Tools

If you aren't using the command line, these online tools provide "Visual Recon":

1. [DNSDumpster](https://dnsdumpster.com/): The best tool for visual mapping. It creates a graph of the entire DNS structure.
2. [MXToolbox](https://mxtoolbox.com/): Best for checking mail records (MX, SPF, DMARC) and blacklists.
3. [SecurityTrails](https://securitytrails.com/): Excellent for **Historical DNS** data (finding old IP addresses).
4. [ViewDNS.info](https://viewdns.info/): A "Swiss Army Knife" for reverse IP and DNS lookups.

---

## 🚀 The OSINT DNS Workflow

### 1. Identify the Infrastructure
> [!DORK] Command Line (Linux/macOS)
> `dig pcampus.edu.np ANY`  
> *Queries all available records at once.*



### 2. Hunting for Subdomains
Look for `CNAME` records. If you see `dev.pcampus.edu.np` pointing to an external service, that service might be misconfigured.

### 3. Email Security Analysis
Check the `TXT` records for SPF strings:
- **Dork:** `site:target.com "v=spf1"`
- **Why?** It lists all IP addresses authorized to send email. This can reveal hidden office IPs or third-party marketing tools.

---

## 📝 DNS Lookup Template
> [!ABSTRACT] Copy this to your Target Note
> 
> ### DNS Recon: {{DOMAIN}}
> - **Primary IP (A):** > - **Mail Servers (MX):** > - **Name Servers (NS):** > - **Security Records (TXT):** > - **Subdomains Found:** >   - [ ] 
>   - [ ] 

---

## 🔗 Internal Links
- [[WHOIS Reconnaissance Guide]]
- [[Google Dorking Cheat Sheet]]
- [[Bug Bounty Methodology]]

> [!TIP] Pro-Tip: DNS Zone Transfers
> Always try `dig axfr @ns1.target.com target.com`. While rare in 2025, a "Zone Transfer" error can leak the **entire** list of subdomains in seconds.