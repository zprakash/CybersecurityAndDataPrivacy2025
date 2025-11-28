# 1️⃣ Introduction

**Tester(s):**  
- Name: Prakash Acharya, Hira Acharya 

**Purpose:**  
- The purpose of this test is to identify vulnerabilities in the Booking System Phase 1 application, focusing specifically on the **registration page**. The goal is to discover security and functionality issues that could lead to unauthorized access, data exposure, or other system misuse.

**Scope:**  
- **Tested components:**  
  - Registration page (`/register`)  
  - Input validation  
  - Server error handling  
  - Session behavior  
  - Security headers  
  - Automated scanning with OWASP ZAP  
- **Exclusions:**  
  - Login page  
  - Booking features  
  - Admin functionality  
  - DoS testing or destructive testing  
- **Test approach:**  
  - **Black-box testing**

**Test environment & dates:**  
- **Start:** *2025-11-28*  
- **End:** *2025-11-28*  
- **Test environment details:**  
  - OS: Windows 11  
  - Application running locally at `http://localhost:8000/`  
  - Browser: Brave 
  - Tools used:  
    - OWASP ZAP 2.16.1  

**Assumptions & constraints:**  
- The application is in an early development phase and expected to have weaknesses.  
- Testing time was limited, so focus was on high-impact issues.  
- No user accounts were required to test registration.  
- All testing was conducted locally and safely.

---

# 2️⃣ Executive Summary

**Short summary:**  
Testing identified several high-risk security vulnerabilities in the registration page, including SQL Injection behavior, missing CSRF protection, weak security headers, and path traversal patterns. These issues expose the application to serious exploitation risks.

**Overall risk level:**  
➡️ **High**

**Top 5 immediate actions:**  
1. Implement server-side validation and parameterized database queries.  
2. Add CSRF protection tokens to all forms.  
3. Add missing security headers (CSP, X-Frame-Options, X-Content-Type-Options).  
4. Fix error handling to avoid exposing server messages.  
5. Harden backend logic to prevent path traversal patterns.

---

# 3️⃣ Severity scale & definitions

|  **Severity Level**  | **Description**                                                                                                              | **Recommended Action**           |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| 🔴 **High** | A serious vulnerability that can lead to full system compromise or data breach (e.g., SQL Injection, Remote Code Execution). | *Immediate fix required*         |
| 🟠 **Medium** | A significant issue that may require specific conditions or user interaction (e.g., XSS, CSRF).                        | *Fix ASAP*                       |
| 🟡 **Low** | A minor issue or configuration weakness (e.g., server version disclosure).                                                   | *Fix soon*                       |
| 🔵 **Info** | No direct risk, but useful for system hardening (e.g., missing security headers).                                            | *Monitor and fix in maintenance* |

---

# 4️⃣ Findings

| ID | Severity | Finding | Description | Evidence / Proof |
|----|----------|----------|-------------|------------------|
| F-01 | 🔴 High | SQL Injection in registration | The `username` field triggered a 500 error when special characters were entered, indicating unsafe SQL handling. | ZAP alert: SQL Injection → parameter `username`. |
| F-02 | 🔴 High | Path Traversal indicators | Registration input allowed path traversal patterns, suggesting weak backend validation. | ZAP alert: Path Traversal → `/register`. |
| F-03 | 🟠 Medium | Missing CSRF protection | No anti-CSRF tokens were present on the registration form. | ZAP alert: Missing Anti-CSRF Tokens. |
| F-04 | 🟠 Medium | Missing security headers | CSP and clickjacking headers were missing across endpoints. | ZAP alerts: CSP Not Set, Anti-Clickjacking Header Missing. |
| F-05 | 🟡 Low | Missing X-Content-Type-Options | Some responses lacked `X-Content-Type-Options: nosniff`, weakening MIME protection. | ZAP alert: Header Missing (nosniff). |

---

# 5️⃣ OWASP ZAP Test Report (Attachment)

**Purpose:**  
- This section includes the complete ZAP scan results in Markdown format.

