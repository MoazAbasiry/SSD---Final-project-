# 🛡️ Full Security Lifecycle Audit & Remediation Report
## Project: Vulnerable Node.js & Express Application
[cite_start]**Organization:** Alexandria National University - Faculty of Engineering [cite: 1, 11]

---

## 👥 1. Development & Security Team
| Name | Student ID | Role |
| :--- | :--- | :--- |
| **Moaz Ahmed** | 2305040 | [cite_start]Lead Security Researcher | [cite: 7]
| **Mosab Magdy** | 2305069 | [cite_start]Remediation Engineer | [cite: 8]
| **Abdelrhman Soliman** | 2305030 | [cite_start]DAST Analyst | [cite: 9]

**Supervised by:** Eng. [cite_start]Mohamed Hatem Abdulkader [cite: 11]

---

## 📖 2. Executive Summary
This comprehensive repository documents the end-to-end security hardening of a Node.js web application. [cite_start]We transitioned the application from a highly vulnerable state (16 major findings) to a production-ready secured state (0 custom rule findings)[cite: 291, 507]. [cite_start]Our approach combined automated DAST, manual exploitation, and SAST-driven remediation[cite: 16, 19].

---

## 🔍 3. Section 1 - DAST Findings (Identification Phase)
[cite_start]The Dynamic Application Security Testing was performed using a two-phased hybrid approach[cite: 15, 16].

### 3.1 Automated Scanning Overview (OWASP ZAP)
[cite_start]We used **OWASP ZAP 2.16.1** to establish a baseline of the security posture[cite: 17, 28].
* [cite_start]**High-Risk Alerts:** 3 critical vulnerabilities identified (XSS, SSTI)[cite: 53, 54].
* [cite_start]**Medium-Risk Alerts:** 4 vulnerabilities related to security configurations[cite: 55].
* [cite_start]**Low Alerts:** 16 findings related to info leakage[cite: 56].

> [cite_start]**[INSERT_IMAGE_HERE: صوره من صفحة 4 في التقرير - ZAP Summary of Alerts]** [cite: 50]

### 3.2 Manual Penetration Testing Results
[cite_start]Using **Postman**, we manually verified 8 critical vulnerabilities (V1 to V8)[cite: 19, 62].

| ID | Endpoint | Vulnerability | Impact |
| :--- | :--- | :--- | :--- |
| **V1** | `/v1/search/id/` | SQL Injection | [cite_start]Critical: Mass data leakage [cite: 63] |
| **V2/3**| `/` | XSS & SSTI | [cite_start]High/Critical: RCE & Script Execution [cite: 63] |
| **V4** | `/v1/user/login` | User Enumeration | [cite_start]Medium: Username discovery [cite: 63] |
| **V5** | `/v1/user/token` | JWT Manipulation | [cite_start]High: Privilege Escalation [cite: 63] |
| **V6** | `/v1/user/:id` | IDOR (BOLA) | [cite_start]High: Accessing other users' data [cite: 63] |
| **V7** | `/v1/test/?url=` | SSRF | [cite_start]High: Internal network exposure [cite: 63] |
| **V8** | `/v1/user/` | Weak Auth | [cite_start]Medium: Trivial password acceptance [cite: 63] |

---

## 🧪 4. Detailed Exploitation Proofs (POCs)

### 4.1 SQL Injection (V1)
* [cite_start]**Exploit:** Appending `1' OR '1'='1` to the ID parameter[cite: 101].
* [cite_start]**Result:** The server returned the entire database of beers (Stella Artois, Heineken, etc.)[cite: 103].

> [cite_start]**[INSERT_IMAGE_HERE: صوره من صفحة 6 في التقرير - V1 SQL Injection Proof]** [cite: 66]

### 4.2 SSTI & XSS (V2 & V3)
* [cite_start]**SSTI Exploit:** Payload `{{7*7}}` evaluated to `49` on the server[cite: 63, 123].
* [cite_start]**XSS Exploit:** `<script>alert('XSS')</script>` executed in the victim's browser[cite: 116].

> [cite_start]**[INSERT_IMAGE_HERE: صوره من صفحة 7 في التقرير - V2/V3 XSS & SSTI Proof]** [cite: 106, 117]

---

## 🤖 5. Section 2 - Semgrep (SAST) Findings
[cite_start]We utilized Semgrep to map DAST findings directly to the source code[cite: 308].

### 5.1 Initial Scan Summary
* [cite_start]**Total Findings:** 16 Critical Findings[cite: 291].
* [cite_start]**Targets Scanned:** 23 JS files[cite: 293].

> [cite_start]**[INSERT_IMAGE_HERE: صوره من صفحة 11 في التقرير - Scan Summary 16 Findings]** [cite: 288]

### 5.2 Custom Rules Evidence
[cite_start]We authored 3 custom rules to target specific project vulnerabilities[cite: 329].
* [cite_start]**JWT Custom Rule:** Found issues at lines 18, 253, and 406[cite: 332].
* [cite_start]**SSTI Custom Rule:** Found issues at lines 17 and 40[cite: 331].

---

## 🛠️ 6. Section 3 - Remediation (The Fixes)
[cite_start]We implemented code-level changes to mitigate all identified risks[cite: 334, 335].

### 📝 Remediation Table
| Vulnerability | Remediation Strategy | File Path |
| :--- | :--- | :--- |
| **SQLi** | [cite_start]Parameterized Queries via Sequelize ORM [cite: 334] | `src/router/routes/user.js` |
| **XSS/SSTI** | [cite_start]Secure `res.render` with data binding [cite: 334] | `src/router/routes/frontend.js` |
| **IDOR** | [cite_start]`req.userId` vs `req.params.id` authorization [cite: 334] | `src/router/routes/user.js` |
| **SSRF** | [cite_start]IP Blacklisting for internal ranges [cite: 334] | `src/router/routes/system.js` |

---

## ✅ 7. Section 4 - Re-test Evidence (Verification)
[cite_start]Post-remediation tests confirm that the application is now secure[cite: 469].

### 7.1 Manual Verification (After Fixes)
1. [cite_start]**SQLi Fixed:** The payload `1' OR '1'='1` now returns an empty array `[]`[cite: 365, 366].
   > [cite_start]**[INSERT_IMAGE_HERE: صوره من صفحة 15 - SQL Injection Failing]** [cite: 339]

2. [cite_start]**User Enumeration Fixed:** Generic error `Invalid email or password` for all failed logins[cite: 374, 375].
   > [cite_start]**[INSERT_IMAGE_HERE: صوره من صفحة 15 - User Enumeration Unified Message]** [cite: 367]

3. [cite_start]**SSRF Fixed:** Blocked with `403 Forbidden` and message `Access to internal resources is forbidden`[cite: 427, 429].
   > [cite_start]**[INSERT_IMAGE_HERE: صوره من صفحة 17 - SSRF Blocked Proof]** [cite: 418]

### 7.2 Automated Verification (Final Semgrep)
* [cite_start]**Standard Rules:** Findings dropped from 16 to 9 (Audit warnings only)[cite: 486, 482].
* [cite_start]**Custom Rules:** **0 Findings**[cite: 507].

> [cite_start]**[INSERT_IMAGE_HERE: صوره من صفحة 19 - Final Semgrep 0 Findings]** [cite: 490]

---

## 🚀 8. How to Install and Run
### Prerequisites
- Node.js (v14+)
- SQLlite/MySQL

### Installation
```bash
npm install
