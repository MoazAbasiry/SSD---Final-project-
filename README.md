# 🛡️ Security Audit & Remediation Project: Node.js/Express App
**Alexandria National University** | **SSD Course Final Project**

---

## 👥 Project Team
* **Moaz Ahmed** - ID: 2305040
* **Mosab Magdy** - ID: 2305069
* **Abdelrhman Soliman** - ID: 2305030

**Presented to:** Eng. Mohamed Hatem

---

## 📖 Executive Summary
This project demonstrates a full security lifecycle for a web application. We identified critical vulnerabilities using a hybrid approach of **Automated DAST (OWASP ZAP)** and **Manual Pentesting (Postman)**. All findings were then verified via **SAST (Semgrep)** and remediated through secure coding practices.

---

## 🛠️ Phase 1: Vulnerability Identification (DAST)
We performed dynamic testing to Establish a security baseline. The following vulnerabilities were identified:

| ID | Vulnerability | OWASP Category | Impact |
| :--- | :--- | :--- | :--- |
| **V1** | SQL Injection | A03: Injection | Critical: Database compromise |
| **V2/3**| XSS & SSTI | A03: Injection | High: RCE & Client-side attacks |
| **V4** | User Enumeration| A01: Broken Access Control | Medium: Info leakage |
| **V6** | IDOR (BOLA) | A01: Broken Access Control | High: Unauthorized data access |
| **V7** | SSRF | A10: SSRF | High: Internal network exposure |

---

## 🔍 Phase 2: Static Analysis (SAST)
We utilized **Semgrep** to map our DAST findings to the source code.

### 📊 Scan Results Summary
* **Initial Scan (Baseline):** **16 High-risk findings** including SQLi and hardcoded secrets.
* **Post-Remediation Scan:** Findings reduced to **9 informational audit warnings**.
* **Custom Ruleset Scan:** **0 Findings** across all targeted patterns.

---

## 🔧 Phase 3: Code Remediation (The Fixes)
We implemented the following code-level fixes to secure the application:

### 1. SQL Injection Fix
* **Strategy:** Replaced raw string concatenation with **Sequelize ORM** models.
* **Code Implementation:** Used `db.user.findOne({ where: { id: req.params.id } })` to ensure automatic parameterization.

### 2. XSS & SSTI Fix
* **Strategy:** Switched from `renderString` (unsafe) to `res.render` with secure data-binding.
* **Impact:** User input is now treated as literal text, preventing script execution.

### 3. Broken Access Control (IDOR)
* **Strategy:** Implemented authorization middleware to verify `req.userId` against the requested resource ID.

---

## ✅ Phase 4: Verification & Re-Testing
Final verification was conducted to ensure the exploits are no longer functional.

### 🧪 Proof of Mitigation (Manual DAST)
1. **SQLi:** Attempting `' OR '1'='1` now returns an empty array `[]` (Safe).
2. **User Enumeration:** Login attempts for non-existent users return unified error messages (Safe).
3. **SSRF:** Internal requests to `localhost` are blocked with **403 Forbidden** (Safe).



---

## 🚀 Installation & Running the Project

### Prerequisites
* Node.js v14+
* MySQL/SQLite

### Setup Environment
1. Clone this repo.
2. Create a `.env` file based on `.env.example`.
3. Run `npm install`.

### Run Security Scans
```bash
# Run baseline Semgrep scan
semgrep --config auto .

# Run custom rules verification
semgrep --config semgrep-rules/ .
