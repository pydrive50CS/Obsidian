---
type: OSINT-Tool
category: Infrastructure
tags: [whois, recon, domain-analysis, osint]
updated: 2025-12-28
---

# 🌐 WHOIS Lookup & Pivot Methodology

WHOIS is a query and response protocol used for querying databases that store the registered users or assignees of an Internet resource (domain name, IP address block, or autonomous system).

---

## 🛠️ Key Data Points to Extract

When performing a WHOIS lookup, look for these specific "pivot points":

1. **Registrant Details:** Name, Organization, and Address (even if redacted, look for a country/state).
2. **Admin/Tech Emails:** Often a direct line to the IT department.
3. **Name Servers (NS):** Reveals the hosting provider (e.g., Cloudflare, AWS, GoDaddy).
4. **Dates:** - *Creation Date:* Is this an old, established domain or a "look-alike" domain created 2 days ago for phishing?
   - *Expiration Date:* Forgotten domains are targets for **Domain Hijacking**.
5. **Registrar:** Where the domain was bought (helps identify the target's preferred vendor).

---

## 🚀 Advanced Pivoting Techniques

### 🔄 Reverse WHOIS
If you find a unique email address or organization name, search for *everything else* they own.
- **Example:** If `admin@aba.bank.com` is used to register a domain, what other domains did that email buy? 
- **Tools:** `Whoxy`, `ViewDNS.info`.

### 📜 WHOIS History
Privacy guards (GDPR) often hide current data. Historical records might show the information before the privacy shield was turned on.
- **Dorking Tip:** `site:whois.com "target.com"` to see if Google has indexed older versions of the records.

---

## 📂 WHOIS Investigation Template
> [!ABSTRACT] How to use
> Copy this block into your specific target note (e.g., `[[pcampus.edu.np]]`) to document your findings.

### Target: {{DOMAIN}}
- **Registrar:** - **Registered On:** - **Expiry Date:** - **Name Servers:** - [ ] NS1:
  - [ ] NS2:
- **Registrant Org:** - **Potential Pivot Email:** ---

## 💻 Command Line Tools (CLI)

If you are on Linux/macOS, use the native terminal for faster results:

```bash
# Basic Lookup
whois target.com

# Filter for specific info
whois target.com | grep -E "Registrant|Email|Name Server"

# Look up an IP Block
whois 1.1.1.1