---
type: CheatSheet
category: OSINT
tags: [dorking, reconnaissance, search-operators, security]
updated: 2025-12-28
---

# 🔍 Google Dorking Master Cheat Sheet

Google Dorking (Google Hacking) involves using advanced operators to query the search engine for information that isn't easily accessible through standard searches.

---

## 🛠️ Core Operators Reference

| Operator | Explanation | Example |
| :--- | :--- | :--- |
| `site:` | Limits results to a specific domain or TLD. | `site:pcampus.edu.np` |
| `inurl:` | Looks for specific strings within the URL path. | `inurl:admin` |
| `intitle:` | Searches for pages with specific words in the HTML title. | `intitle:"Index of"` |
| `filetype:` | Filters results by file extension (e.g., pdf, log, sql). | `filetype:env` |
| `intext:` | Searches for specific strings anywhere on the page content. | `intext:"password"` |
| `cache:` | Displays Google's cached version of a page (useful if site is down). | `cache:target.com` |
| `link:` | Finds pages that link to the specified URL. | `link:target.com` |
| `*` | Wildcard operator—matches any word or phrase. | `site:*.edu.np` |

---

## 🚀 Advanced Recon Combinations

### 📂 1. Directory Listing & Information Disclosure
These dorks find servers that have "Directory Listing" enabled, exposing the folder structure.

> [!DANGER] Sensitive Directory Dork
> `intitle:"index of" "parent directory"`
> *Standard signature for open web directories.*

* **Find WordPress Uploads:** `site:target.com inurl:wp-content/uploads/`
* **Server Logs:** `filetype:log intext:"error" OR intext:"debug"`
* **Git Repositories:** `inurl:/.git` (Finding exposed `.git` folders can lead to full source code recovery).

---

### 🔑 2. Credential & Config Hunting
Used to find hardcoded passwords, API keys, and database credentials.

> [!CAUTION] Environment Files
> `filetype:env "DB_PASSWORD" OR "SECRET_KEY"`
> *Targeting `.env` files used in modern web frameworks like Laravel or Node.js.*

* **SQL Backups:** `filetype:sql "INSERT INTO" "users" "password"`
* **Publicly Indexed SSH Keys:** `filetype:id_rsa OR filetype:id_dsa`
* **Config Files:** `filetype:config "connectionString"`

---

### 🌐 3. Infrastructure & Login Portals
Mapping the management surface of the organization.

* **Admin Portals:** `site:target.com inurl:admin OR inurl:login OR inurl:manage`
* **Common Panels:** `intitle:"Cpanel" OR intitle:"Plesk" OR intitle:"phpMyAdmin"`
* **Camera Feeds (IoT):** `inurl:"/view.shtml" OR "Network Camera"`

---

## 📝 Best Practices for OSINT Professionals

1. **Use a VPN:** Frequent dorking triggers Google's automated bot detection (CAPTCHAs).
2. **Combine Operators:** The power of a dork lies in stacking.
   * *Example:* `site:target.com filetype:php inurl:id=` (Searching for potential SQLi entry points).
3. **Check the Cache:** If a page was recently taken down due to a leak, use `cache:url` to see if Google still has the data stored.

---

## 🔗 Related Notes
- [[OSINT Reconnaissance - pcampus.edu.np]]
- [[Bug Bounty Methodology]]
- [[Internal Tools Checklist]]

> "The more you know about your target, the easier the penetration becomes."