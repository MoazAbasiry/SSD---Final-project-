# 🛡️ SSD Project: Security Audit & Remediation
**Alexandria National University** | **Faculty of Engineering**

---

## 👥 Development Team
* **Moaz Ahmed** - ID: 2305040
* **Mosab Magdy** - ID: 2305069
* **Abdelrhman Soliman** - ID: 2305030

**Presented to:** Eng. Mohamed Hatem

---

## 📋 Project Overview
This project demonstrates a comprehensive security analysis and remediation of a Node.js/Express application. We utilized a hybrid approach involving **OWASP ZAP** for automated DAST and **Semgrep** for automated SAST.

---

## 🛠️ Remediated Vulnerabilities (V1 - V8)
Based on our technical audit, we successfully mitigated the following vulnerabilities:

| ID | Vulnerability Name | Mitigation Strategy | Status |
| :--- | :--- | :--- | :--- |
| **V1** | **SQL Injection** | Implemented Sequelize ORM Parameterized Queries | ✅ Fixed |
| **V2/3**| **XSS & SSTI** | Switched to secure data-binding via `res.render` | ✅ Fixed |
| **V4** | **User Enumeration**| Unified error messages for failed auth attempts | ✅ Fixed |
| **V6** | **IDOR (BOLA)** | Added strict object-level authorization checks | ✅ Fixed |
| **V7** | **SSRF** | Implemented IP Blacklisting for internal infrastructure | ✅ Fixed |
| **V8** | **Weak Password** | Enforced 8-char policy & Bcrypt hashing | ✅ Fixed |

---

## 🔍 Verification Evidence
Verification was performed using automated scans and manual exploit attempts:

* **Semgrep Baseline Cleanup:** Findings reduced from **16 to 9** informational alerts.
* **Custom Ruleset Validation:** Achieved **0 findings** using targeted custom rules.
* **Manual DAST Verification:** Successful mitigation of SQLi (Empty Array response) and XSS (Literal rendering).

---

## 🚀 How to Run
### 1. Installation
```bash
npm install
