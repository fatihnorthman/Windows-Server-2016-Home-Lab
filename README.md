## 📦 Phase 10: Endpoint State Orchestration & Identity Hardening

> *"True enterprise administration is not managing users; it is orchestrating state. By centralizing data and locking down endpoints, we shift from reactive troubleshooting to proactive infrastructure governance."*

**Executive Summary:** This phase focuses on standardizing the endpoint user experience, centralizing critical user data for backup purposes, and hardening identity security. Key implementations include configuring Folder Redirection for data centralization, enforcing corporate identity via locked wallpapers, and establishing strict domain-level password policies to mitigate brute-force and dictionary attacks.

---

### 📂 I. Centralized Data Management (Folder Redirection)
**Objective:** Prevent data loss caused by hardware failure on endpoints by silently redirecting user profile folders (e.g., Desktop) to a centralized, redundant file server.

* **Infrastructure Preparation (Storage & Permissions):**
  * Created a dedicated network share (`\\ServerName\RedirectedFolders$`).
  * Configured rigorous NTFS and Share permissions to ensure users can only write to their respective directories, maintaining strict data isolation.
* **GPO Implementation:**
  * **Path:** `User Configuration ➔ Policies ➔ Windows Settings ➔ Folder Redirection ➔ Desktop`
  * **Setting:** `Basic - Redirect everyone's folder to the same location`
  * **Target Folder Location:** Set to the UNC path of the centralized share.
* **Architectural & Security Rationale:**
  * 🔴 **Exclusive Rights Disabled:** Explicitly unchecked *"Grant the user exclusive rights to Desktop"*. 
    * *SysAdmin Rationale:* By default, Windows locks out administrators from redirected folders. Disabling this ensures Domain Administrators and automated Backup Service Accounts retain Read/Write access to user data for disaster recovery and compliance auditing.
  * 🟢 **Legacy Compatibility:** Enabled *"Also apply redirection policy to Windows 2000, 2000 Server, XP, and 2003 operating systems"* to maintain backward compatibility in mixed-OS environments and prevent edge-case failures.

---

### 🖼️ II. Corporate Identity Standardization (UI Lockdown)
**Objective:** Enforce corporate branding on all domain-joined machines and prevent unauthorized endpoint modifications.

* **Implementation Strategy:**
  * Hosted the official corporate wallpaper on a highly available, read-only network share.
  * Created a specific GPO to push the background image (`User Config ➔ Admin Templates ➔ Desktop ➔ Desktop ➔ Desktop Wallpaper`).
  * Configured supplementary policies to lock the Control Panel personalization settings, explicitly preventing users from bypassing or changing the enforced wallpaper.
* **Outcome:** Achieved a unified corporate aesthetic across all departments, mitigating inappropriate desktop backgrounds and standardizing the visual environment.

---

### 🔐 III. Identity & Access Hardening (Password Policies)
**Objective:** Fortify the Active Directory authentication mechanism against credential-based attacks (e.g., brute-force, password spraying, rainbow tables).

* **GPO Implementation (Default Domain Policy):**
  * **Path:** `Computer Configuration ➔ Policies ➔ Windows Settings ➔ Security Settings ➔ Account Policies ➔ Password Policy`
* **Enforced Parameters:**
  * **Password must meet complexity requirements: [Enabled]** * *Enforcement:* Forces a mix of uppercase, lowercase, numbers, and special characters, eliminating simple dictionary passwords.
  * **Minimum password length:** * *Enforcement:* Established a strict minimum character limit (e.g., 8-12 characters) to exponentially increase the time required for offline cracking attempts.
  * **Enforce password history:** * *Enforcement:* Prevented users from continuously cycling through previously used passwords (e.g., remembering the last 5 passwords).
* **Outcome:** Significantly elevated the domain's security posture by ensuring all authenticated accounts adhere to enterprise-grade credential standards.
