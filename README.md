# 📊Networkwalks_B083D_Week-4-cybersecurity-Lab-setup.
## 🖤Penetration Testing Project_Mediroza Hospital_Batch B083| Week_4.
## Target: https://medirozahospital.com

</div>

<p align="center">
    <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
    <img src="https://img.shields.io/badge/Ver-Virtualbox%20v7.2-0070C0?style=flat-square&labelColor=000000" />
    <img src="https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
    <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" />
    <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" />
    <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
    <img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" />
    <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
    <img src="https://img.shields.io/badge/Kali%20Linux-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" />
    <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" />
    <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
    <img src="https://img.shields.io/badge/Rahul%20Rathore%20Networkeng.-C00000?style=flat-square" />
</p>

---

## 📌 Project Overview
This repository contains a technical write-up for a simulated penetration testing engagement against a healthcare web application (Mediroza Hospital). The objective of this project was to identify vulnerabilities in the external infrastructure, achieve initial access, extract sensitive data, and crack encrypted files.

**Disclaimer:** This assessment was performed in a controlled, authorized training environment. All personally identifiable information (PII) mentioned in this write-up is simulated and fictional.

## 🛠️ Skills & Tools Demonstrated
* **Reconnaissance:** `whois`, `nmap`, `whatweb`, directory enumeration.
* **Web Exploitation:** SQL Injection (SQLi) Authentication Bypass, Insecure Direct Object Reference (IDOR).
* **Cryptography/Cracking:** Hash extraction, dictionary attacks, `hashcat`.
* **Data Exfiltration:** interacting with exposed directories via `curl`.

---

## 🚀 Attack Narrative & Methodology

### Phase 1: Reconnaissance
The engagement began with passive and active reconnaissance to map the target's attack surface. 
* Identified the hosting provider and DNS details using `whois` and subdomain enumeration.
* Discovered the web server was running LiteSpeed via `whatweb`.
* Executed aggressive `nmap` scans, identifying open HTTP (80, 8080) and HTTPS (443) ports.

### Phase 2: Initial Access via SQL Injection
The primary attack vector was the Patient Portal login page. Initial tests using the classic `' OR '1'='1` payload returned a specific SQL syntax error, revealing that a backend filter was stripping the `OR` keyword (a common blacklisting rule). 

To bypass this filter, I adjusted the payload to a standard comment-out bypass:
```sql
admin'-- -
```
This payload successfully bypassed authentication without requiring a password, granting access to the portal where three password-protected patient pathology reports (PDFs) were hosted and subsequently downloaded.

### Phase 3: Data Extraction and Password Cracking
The downloaded PDF reports were encrypted. To access the contents, I extracted the PDF hashes and performed offline cracking.
1. Extracted hashes using an online hash extraction tool.
2. Saved the extracted hashes locally (e.g., `$pdf$2*3*128*...`).
3. Utilized password cracking tools (NetworkWalks built-in tools and `hashcat`) to recover the passwords.

**Hashcat Example:**
```bash
hashcat -m 10500 pdf_hash.txt /usr/share/wordlists/rockyou.txt --force -w 1
```
*Results:* All three PDF passwords were recovered due to weak password policies (e.g., `123456`, `password`, and simple symbol strings).

### Phase 4: Directory Enumeration & Database Leak
During the reconnaissance phase, I enumerated the `robots.txt` file, which revealed a disallowed path: `/old/`.
```bash
curl -s https://[TARGET_DOMAIN]/robots.txt
```
Sending a `HEAD` request confirmed the directory was publicly accessible (HTTP 200). Navigating to the directory revealed that **Directory Indexing** was enabled on the LiteSpeed server, exposing a full SQL database backup.

I extracted the backup using `curl`:
```bash
curl -s -o mediroza_db_backup_2019.sql https://[TARGET_DOMAIN]/old/mediroza_db_backup_2019.sql
```
Analyzing the `.sql` dump revealed highly sensitive, unencrypted tables containing staff salaries, national IDs, and shareholder details.

---

## 🛡️ Vulnerability Summary & Mitigations
This lab highlighted several critical security misconfigurations:
1. **Authentication Bypass (SQLi):** Mitigated by implementing Parameterized Queries (Prepared Statements) instead of relying on weak blacklist filters.
2. **Sensitive Data Exposure:** Mitigated by removing public-facing database backups and storing them on secure internal servers.
3. **Security Misconfiguration:** Mitigated by disabling directory indexing on the web server to prevent unauthorized file browsing.
4. **Weak Cryptography:** Mitigated by enforcing strict, high-entropy password policies for encrypting confidential documents.

# 👤 Author
**Rahul Rathore**

Cybersecurity Starter

LinkedIn: www.linkedin.com/in/rahul-rathore91

# Project Imformation

**Program Name:** Cybersecurity at Networkwalks | **Week: 04 | Project:** Penetration Testing Project Mediroza Hospital | 
**Repository:** GitHub

## Penetration Report.↘️
PENETRATION TESTING REPORT.docx

<img width="2880" height="1612" alt="Medirozahospital password find" src="https://github.com/user-attachments/assets/9f02d461-052e-48f6-9708-89ed99183c9a" />

<img width="2880" height="1612" alt="password brich" src="https://github.com/user-attachments/assets/42aca1be-037d-4ccd-a9e8-9097342d63ee" />
<img width="2880" height="1612" alt="VirtualBox_kali-linux-2026 2-virtualbox-amd64_01_10_2026_17_37_38" src="https://github.com/user-attachments/assets/d2f9a4fe-fabf-46c6-9223-ac51fc4d555e" />
<img width="2880" height="1612" alt="VirtualBox_kali-linux-2026 2-virtualbox-amd64_01_10_2026_17_38_06" src="https://github.com/user-attachments/assets/a3c3c20e-a7c3-4d58-8f44-e5543d6c0655" />
<img width="2880" height="1612" alt="VirtualBox_kali-linux-2026 2-virtualbox-amd64_01_10_2026_17_38_25" src="https://github.com/user-attachments/assets/e9cd6ac7-3a60-453f-8e0a-0eced126cede" />
<img width="2880" height="1612" alt="VirtualBox_kali-linux-2026 2-virtualbox-amd64_01_10_2026_17_39_33" src="https://github.com/user-attachments/assets/924e063f-981b-4c82-9257-9f5f6ac923eb" />
<img width="2880" height="1612" alt="VirtualBox_kali-linux-2026 2-virtualbox-amd64_01_10_2026_17_39_52" src="https://github.com/user-attachments/assets/26b033c9-d4fa-4316-b3a5-e2635813be1b" />
<img width="2880" height="1612" alt="VirtualBox_kali-linux-2026 2-virtualbox-amd64_01_10_2026_17_40_01" src="https://github.com/user-attachments/assets/b5cabc17-057e-41f6-bab2-1db8cbeffb74" />
![Uploading VirtualBox_kali-linux-2026.2-virtualbox-amd64_01_10_2026_17_40_01.png…]()
![Uploading VirtualBox_kali-linux-2026.2-virtualbox-amd64_01_10_2026_17_48_23.png…]()












