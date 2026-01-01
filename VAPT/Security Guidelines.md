
## Frameworks and Compliance


### Frameworks:
GDPR: Personal data (PII)
ISO 27001: ISMS
NIST: Security guidelines

---

### Compliance
Following and implementing the rules defined by the legal frameworks
doing what framework says and documenting everything

VAPT examples:
Sign NDA
Creating Test Plan
Documenting Risks and fixes

Framework = rules
Compliance = Following the Rules

---
### GDPR
data protection and privacy act defined in 2018 by EU

GDPR Steps
1.Identify personal data

company must identify:
	what personal data they collect
	where it is stored
	who can access it

2. define purpose and lawful basis
	Data must be collected for a clear legal reason such as:
		User consent
		Legal obligation
		contract requirement
		
3. Secure the data
		Secure servers, databases, applications
		perform VA
		regular updates and patching
Where valid?
IT companies, universities, banks/fintech,, e-commerce platforms, SAAS and Cloud Service Providers


How? (General practice)
update privacy policy
encrypt data
collect minimum information
train employees
regular audits

---
### NIST SP 800-115


Q.A nepal based IT company is hired to perform VAPT on onine healthcare platforn used by european patients.

find vulnerability that allows access to patient medical records
wants to download the database to prove impact
plans to stat testing immediately t meet a tight deadline(1-2 weeks)
has not clearly documented risk remediation steps

Group 1- GDPR team
Is downloading patient data allowed?
What GDPR principles apply here?
What should testers do to stay GDPR compliant?


Group 2- NIST Team
Group 3- ISO Team

---

- Passive reconnaissance
- Active reconnaissance

### OSINT

collecting useful information from publicly available sources without directly accessing, scanning or attacking the target system
gather information of a company without touching its servers
- LinkedIn
- google dorking
- official website to gather domain names, emails etc

#### Common OSINT Sources

- Search engines
- "site:linkedin.com"
- social media platforms(user generated content, SOCMINT capabilities, Real World Applications)
- company websites: footprints job openings tech stack etc
- job portals
- code repositories
- forums

---
target: Pulchowk Campus
domain: pcampus.edu.np
category: Reconnaissance
status: #OSINT/Active
tags: [dorking, recon, pentest, osint]
created: 2025-12-28
---

# 🕵️ OSINT Reconnaissance: pcampus.edu.np

## 📌 Project Overview
- **Objective:** Passive reconnaissance using Google Dorking.
- **Scope:** Publicly available information only. No active scanning or intrusive tools.
- **Methodology:** Systematic indexing of subdomains, sensitive files, and infrastructure details.

---

## 🛠️ Reconnaissance Phases

### 1. Subdomain & Asset Discovery
Identifying the digital perimeter and hidden departments.

> [!NOTE] Dork: `site:pcampus.edu.np -www`
> Filters out the main landing page to find sub-orgs (e.g., `doece.pcampus.edu.np`).

> [!TIP] Dork: `site:*.pcampus.edu.np`
> Wildcard search for deep-level subdomains.

- [ ] Check for development or staging sites (e.g., `dev.*`, `test.*`).
- [ ] Map departmental portals.

---

### 2. Document & Data Leakage
Searching for sensitive information stored in common file formats.

| File Type | Dork Query                          | Target Content                         |
| :-------- | :---------------------------------- | :------------------------------------- |
| **PDF**   | `site:pcampus.edu.np filetype:pdf`  | Syllabus, Admission lists, Memos       |
| **Excel** | `site:pcampus.edu.np filetype:xlsx` | Budgeting, Student data, Contact lists |
| **Word**  | `site:pcampus.edu.np filetype:doc`  | Internal policies, Project reports     |

> [!WARNING] High Priority Dork
> `site:pcampus.edu.np intext:"confidential" OR intext:"internal use only"`

---

### 3. Infrastructure & Auth Discovery
Finding login portals and technology fingerprints.

- **Login Portals:** `site:pcampus.edu.np inurl:login OR inurl:admin OR inurl:portal`
- **Directory Listing:** `site:pcampus.edu.np intitle:"index of"`
- **Technology Stack:** `site:pcampus.edu.np ext:php OR ext:aspx`

---

### 4. Configuration & Credential Leakage
The "Gold Mine" of Dorking—finding developer mistakes.

- **Environment Files:** `site:pcampus.edu.np filetype:env OR "DB_PASSWORD"`
- **SQL Backups:** `site:pcampus.edu.np filetype:sql OR filetype:bak`
- **Config Files:** `site:pcampus.edu.np inurl:config.php OR inurl:settings.php`

---

## 📝 Findings Log

| Date | Finding | Severity | Source URL |
| :--- | :--- | :--- | :--- |
| 2025-12-28 | Initial Recon Started | Info | N/A |
| | | | |

---
---
type: Dork-List
target_tld: .com.np
category: OSINT
tags: [dorking, nepal, reconnaissance]
---

# 🇳🇵 Google Dorks for .com.np Domains

These dorks are designed to map the Nepalese web space, focusing on government, commercial, and educational sectors within the `.np` TLD.

---

## 🌐 Broad Scoping
Find all indexed sites under the commercial Nepalese TLD.

> [!DANGER] The Master Query
> `site:.com.np`
> *Lists every website indexed by Google ending in .com.np.*

* **Exclude WWW:** `site:.com.np -www` (Finds subdomains like `mail.company.com.np`)
* **Government Sites:** `site:.gov.np` (Specifically for Nepal Government portals)
* **Educational Sites:** `site:.edu.np` (Universities and colleges)

---

## 📂 Sensitive Data Discovery
Searching for exposed documents within Nepalese infrastructure.

| Goal | Dork Query |
| :--- | :--- |
| **PDF Documents** | `site:.com.np filetype:pdf` |
| **Excel (Data Sheets)** | `site:.com.np filetype:xlsx` |
| **SQL Backups** | `site:.com.np filetype:sql` |
| **Config Files** | `site:.com.np filetype:env OR ext:conf` |

---

## 🔑 Access Points & Vulnerabilities
Locating login portals or misconfigured servers in the `.np` space.

* **Admin Panels:** `site:.com.np inurl:admin OR inurl:login`
* **Directory Listing:** `site:.com.np intitle:"index of"`
* **WordPress Specific:** `site:.com.np inurl:wp-content`
* **CCTV/IoT Devices:** `site:.com.np inurl:view.shtml`

---

## 📝 Search Filtering Tips

> [!TIP] Combine with Keywords
> To find specific sectors, add keywords to the dork:
> * `site:.com.np "bank"` (Finds all banking sites in Nepal)
> * `site:.edu.np "result"` (Finds student result portals)

---

## 🔗 Related
- [[Google Dorking Cheat Sheet]]
- [[WHOIS Reconnaissance Guide]]
## 🔗 Internal Links & References
- [[Google Dorking Cheat Sheet]]
- [[OSINT Tools Checklist]]
- [[Remediation Plan Template]]
- [[WhoIs]]

> [!INFO] Footnote
> This report was generated for educational purposes and follows ethical OSINT guidelines.










