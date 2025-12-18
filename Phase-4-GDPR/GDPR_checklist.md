
# GDPR Compliance Checklist – Web-based Booking System

| **Result** | **Personal data mapping and minimization** |
| :----: | :--- |
| ✅ | All personal data collected and processed identified (emails, birthdates, tokens, IPs) |
| ✅ | Only necessary personal data is collected (no extra fields beyond registration & booking) |
| ✅ | Age is recorded (birthdate) to verify bookers are over 15 |

---

| **Result** | **User registration and management** |
| :----: | :--- |
| ✅ | Registration form includes consent for data processing (terms_accepted) |
| ⚠️ | Users can view/edit some data; deletion of own account not fully visible yet |
| ✅ | Admin can delete a reserver (Right to be forgotten) |
| ✅ | Underage registration blocked (users under 15 cannot book) |

---

| **Result** | **Booking visibility** |
| :----: | :--- |
| ✅ | Bookings visible to non-logged-in users only at resource level (no personal data shown) |
| ✅ | Names, emails, and personal data of bookers are not exposed publicly |

---

| **Result** | **Access control and authorization** |
| :----: | :--- |
| ✅ | Only admins can add/modify/delete resources and bookings |
| ✅ | Role-based access control implemented (reserver vs admin) |
| ✅ | Admin privileges limited to necessary operations; cannot abuse personal data |

---

| **Result** | **Privacy by Design Principles** |
| :----: | :--- |
| ✅ | Privacy by Default implemented (minimal data collected by default) |
| ✅ | Logs implemented without unnecessary personal data (e.g., admin logs link to IDs, not emails) |
| ✅ | Forms designed with data protection in mind (secure login, minimal fields) |

---

| **Result** | **Data security** |
| :----: | :--- |
| ✅ | CSRF, XSS, SQL injection protections appear implemented |
| ✅ | Passwords hashed securely (bcrypt) |
| ⚠️ | Backup/recovery GDPR compliance not directly observable |
| ❌ | Personal data storage location uncertain (EU vs non-EU) |

---

| **Result** | **Data anonymization and pseudonymization** |
| :----: | :--- |
| ✅ | Personal data pseudonymized where possible (reserver_token used instead of email in reservations) |
| ✅ | Anonymization/pseudonymization used without losing booking functionality |

---

| **Result** | **Data subject rights** |
| :----: | :--- |
| ⚠️ | Users cannot download all personal data directly (no visible export function) |
| ⚠️ | No explicit interface for requesting deletion of personal data (besides admin deletion) |
| ✅ | Users can withdraw consent by not registering or deleting account via admin |

---

| **Result** | **Documentation and communication** |
| :----: | :--- |
| ✅ | No visible Privacy Policy page (/privacypolicy not implemented yet) |
| ⚠️ | Admins/developers have limited documentation observed |
| ⚠️ | No documented data breach response process visible |

---

**Symbols used:**  
✅ Pass  
❌ Fail  
⚠️ Attention required
