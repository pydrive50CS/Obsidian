---
type: Methodology
category: Web Security
tags: [bug-bounty, pentesting, workflow, web-app]
updated: 2025-12-28
---

# 🎯 Bug Bounty Hunting Methodology

This document outlines a repeatable framework for hunting vulnerabilities on large-scale targets.

---

## 🏗️ Phase 1: Reconnaissance (The Wider the Net...)
Before attacking, map the entire surface.

### 1.1 ASN & IP Discovery
- [ ] Identify ASN (Autonomous System Number) using `bgp.he.net`.
- [ ] Reverse WHOIS to find related domains.
- [ ] **Tools:** `Amass`, `Whois`, `ViewDNS`.

### 1.2 Subdomain Enumeration
- [ ] **Passive:** Use `Subfinder`, `Assetfinder`, and Google Dorks ([[Google Dorking Cheat Sheet]]).
- [ ] **Active:** Brute-force using `ffuf` or `Gobuster` with a high-quality wordlist (e.g., `2m-subdomains.txt`).
- [ ] **Permutations:** Use `altdns` to find hidden subdomains like `dev-api.target.com`.

---

## 🕵️ Phase 2: Analysis & Fingerprinting
Once you have the list of subdomains, identify what is "alive" and what it's running.



### 2.1 Port Scanning & HTTP Probing
- [ ] Run `httpx` to find live web servers and their status codes (200, 403, 401).
- [ ] Use `naabu` for fast port scanning.

### 2.2 Tech Stack Identification
- [ ] Identify CMS, Frameworks, and Server versions.
- [ ] **Tools:** `Wappalyzer` (CLI/Extension), `BuiltWith`, or `Nuclei` (technologies template).

---

## 🛠️ Phase 3: Vulnerability Hunting (The Deep Dive)
Divide your testing into logical categories.

### 3.1 Authentication & Session Management
- [ ] Test for **Broken Authentication** (default credentials, password reset flaws).
- [ ] **IDOR (Insecure Direct Object Reference):** Can I change `user_id=100` to `user_id=1` in the API?
- [ ] Check for JWT misconfigurations (None algorithm, weak secrets).

### 3.2 Input Validation
- [ ] **XSS (Cross-Site Scripting):** Search for reflection points in search bars or profile fields.
- [ ] **SQLi (SQL Injection):** Test parameters with `'` or `"`—use `sqlmap` if a hit is suspected.
- [ ] **SSRF (Server-Side Request Forgery):** Can the server make a request to internal metadata (`169.254.169.254`)?

### 3.3 Logical Flaws (High Bounties)
- [ ] **Race Conditions:** Can I redeem a coupon twice by sending simultaneous requests?
- [ ] **Price Manipulation:** Can I change the price of an item in the checkout request?

---

## 📤 Phase 4: Reporting & Documentation
A bug isn't worth anything unless you can explain it clearly.

> [!IMPORTANT] Report Structure
> 1. **Title:** Clear and concise (e.g., *Reflected XSS on [Endpoint] via [Parameter]*).
> 2. **Description:** High-level summary of the bug.
> 3. **Impact:** Why does this matter? (e.g., "An attacker can steal user cookies").
> 4. **Steps to Reproduce:** Bulleted list (1, 2, 3) that a developer can follow.
> 5. **Remediation:** How to fix it.

---

## 🧰 Essential Toolset
- **Proxy:** Burp Suite Professional (The industry standard).
- **Fuzzing:** `ffuf`, `wfuzz`.
- **Scanning:** `Nuclei` (Template-based scanning).
- **Automation:** `Project Discovery` suite.

---

## 📈 Next Steps
- [ ] Set up a dedicated VPS for scanning.
- [ ] Create a "Golden Wordlist" for directory discovery.
- [ ] Monitor [[Google Dorking Cheat Sheet]] for new dork patterns.

> [!TIP] Pro-Tip
> Always check the **Program Policy** on HackerOne/Bugcrowd before starting. Stay in scope!