# 🛡️ Full Security Lifecycle Audit & Remediation Report
## Project: Vulnerable Node.js & Express Application
**Organization:** Alexandria National University - Faculty of Engineering

---

## 👥 1. Development & Security Team
* **Moaz Ahmed** - ID: 2305040
* **Mosab Magdy** - ID: 2305069
* **Abdelrhman Soliman** - ID: 2305030

**Presented to:** Eng. Mohamed Hatem Abdulkader

---

## 📖 2. Executive Summary
This comprehensive repository documents the end-to-end security hardening of a Node.js web application. We transitioned the application from a highly vulnerable state (16 major findings) to a production-ready secured state (0 custom rule findings). Our approach combined automated DAST, manual exploitation, and SAST-driven remediation.

---

## 🔍 3. Section 1 - DAST Findings (Identification Phase)
The Dynamic Application Security Testing (DAST) was performed using a two-phased hybrid approach.

### 3.1 Automated Scanning Overview (OWASP ZAP)
We used **OWASP ZAP 2.16.1** to establish a baseline of the security posture.
* **High-Risk Alerts:** 3 critical vulnerabilities (XSS, SSTI).
* **Medium-Risk Alerts:** 4 vulnerabilities related to configurations.
* **Low & Informational Alerts:** 16 findings providing insights into best practices.

![ZAP Summary of Alerts](./screenshots/zap_summary.png)
*Figure 1: OWASP ZAP Executive Summary.*

### 3.2 Manual Penetration Testing Results
Using **Postman**, we manually verified 8 critical vulnerabilities (V1 to V8).

| ID | Endpoint | Vulnerability | OWASP Category | Short Impact |
| :--- | :--- | :--- | :--- | :--- |
| **V1** | `/v1/search/id/` | SQL Injection | A03: Injection | Critical: Mass data leakage |
| **V2** | `/?message=` | Reflected XSS | A03: Injection | High: Client-side script execution |
| **V3** | `/?message=` | SSTI | A03: Injection | Critical: Remote Code Execution (RCE) |
| **V4** | `/v1/user/login` | User Enumeration | A01: Broken Access Control | Medium: Account status discovery |
| **V5** | `/v1/user/token` | JWT Manipulation | A02: Crypto Failures | High: Privilege Escalation |
| **V6** | `/v1/user/:id` | IDOR (BOLA) | A01: Broken Access Control | High: Unauthorized data access |
| **V7** | `/v1/test/?url=` | SSRF | A10: SSRF | High: Internal network exposure |
| **V8** | `/v1/user/` | Identification Failure | A07: Auth Failures | Medium: Weak password acceptance |

---

## 🧪 4. Detailed Exploitation Proofs (POCs)

### 4.1 SQL Injection (V1)
* **Exploit:** Appending the malicious payload `1' OR '1'='1` to the ID parameter.
* **Result:** The database evaluated the statement as true for every row, bypassing intended filters.
* **Observation:** The server leaked all product records including Stella Artois and Heineken.

![V1 SQL Injection Proof](./screenshots/sqli_exploit.png)
*Figure 2: Mass data disclosure via SQL Injection.*

### 4.2 SSTI & XSS (V2 & V3)
* **SSTI Proof:** Submitting `{{7*7}}` in the message field; the server returned `49`.
* **XSS Proof:** Injected `<script>alert('XSS')</script>` which executed directly in the browser.

![V2/V3 XSS & SSTI Proof](./screenshots/ssti_proof.png)
*Figure 3: Server-Side Template Injection results.*

---

## 🤖 5. Section 2 - Semgrep (SAST) Findings
We utilized Semgrep to map DAST findings directly to the source code for targeted patching.

### 5.1 Initial Scan Summary
* **Total Findings:** 16 Critical Findings.
* **Key Targets:** Hardcoded secrets in `user.js` and insecure templates in `frontend.js`.

![Scan Summary 16 Findings](./screenshots/semgrep_initial.png)
*Figure 4: Initial SAST scan results with 16 findings.*

### 5.2 Custom Rules Evidence
We authored 3 custom rules to demonstrate advanced detection.
* **JWT Custom Rule:** Identified hardcoded secrets at lines 18, 253, and 406.
* **SSTI Custom Rule:** Identified unsafe nunjucks usage at lines 17 and 40.

---

## 🛠️ 6. Section 3 - Remediation (The Fixes)
We implemented enterprise-grade code changes to mitigate all identified risks.

| Vulnerability | Remediation Strategy | Security Impact |
| :--- | :--- | :--- |
| **SQLi** | Parameterized Queries via Sequelize ORM | Prevents malicious SQL execution |
| **XSS/SSTI** | Secure `res.render` with data binding | Inputs treated as literal text |
| **IDOR** | `req.userId` vs `req.params.id` check | Ensures users only access own data |
| **SSRF** | IP Blacklisting for internal ranges | Prevents proxy attacks on internal infra |
| **Weak Password**| Bcrypt Hashing & 8-char policy | Protects credentials even if leaked |

---

## ✅ 7. Section 4 - Re-test Evidence (Verification)
Post-remediation tests confirm that the application is now secure.

### 7.1 Manual Verification (After Fixes)
1. **SQLi Fixed:** The payload `1' OR '1'='1` now returns an empty array `[]`.
   ![SQL Injection Failing](./screenshots/sqli_fixed.png
