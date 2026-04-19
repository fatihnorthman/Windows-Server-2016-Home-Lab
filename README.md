## 🔐 Phase 4: Endpoint Integration & Identity Security (IAM)

> *"Trust is good, but explicit control is better. Securing the perimeter starts with securing the identity."*

**Executive Summary:** This phase marks the critical transition from basic infrastructure deployment to active **Identity and Access Management (IAM)**. It documents the successful integration of physical endpoints into the `northman.com` domain and the enforcement of strict *"Zero Trust"* and *"Principle of Least Privilege" (PoLP)* security policies.

---

### 🚀 I. Core Operations: The First Handshake
**Objective:** Securely integrate physical Windows 10 endpoints into the centralized Active Directory architecture.

| Action Phase | Protocol / Command | Outcome / Technical Validation |
| :--- | :--- | :--- |
| **1. Network Config** | Static IP & DNS set to `192.168.1.200` | Client successfully queried `_ldap._tcp.dc._msdcs` SRV records to locate the DC. |
| **2. Domain Join** | `System Properties` ➔ `Domain` | Transitioned from local SAM authentication to domain-wide **Kerberos v5**. |
| **3. Security Audit** | `lusrmgr.msc` vs. Domain Credentials | Verified that standard domain users lack unauthorized local administrator privileges. |

---

### 🛡️ II. IAM Field Logs: System Hardening Policies
The following security policies were deployed via Active Directory Users and Computers (ADUC) to mitigate internal threats and automate identity lifecycles.

#### 🛑 Policy #01: The Restricted Terminal (Anti-Lateral Movement)
* **Threat Vector:** Compromised credentials being used to traverse the network (e.g., a standard user logging into a critical HR or Server machine).
* **Implementation:** `User Properties` ➔ `Account` ➔ `Log On To...`
* **Technical Mechanism:** Modified the AD `logonWorkstation` attribute.
* **Status:** `[ ACTIVE ]` — Users are explicitly whitelisted to authenticate *only* on their assigned NetBIOS computer names.

#### 🌙 Policy #02: The Night Shift Lockout (Time-Based Access)
* **Threat Vector:** Off-hours data exfiltration or unauthorized physical access to office terminals.
* **Implementation:** `User Properties` ➔ `Account` ➔ `Logon Hours`
* **Technical Mechanism:** Configured the `logonHours` matrix.
* **Status:** `[ ACTIVE ]` — The system cryptographically denies Kerberos Ticket Granting Tickets (TGTs) outside of authorized business hours (e.g., 18:00 to 08:00).

#### ⏳ Policy #03: Automated Lifecycle Management (The Kill-Switch)
* **Threat Vector:** "Ghost Accounts" left behind by departed contractors or interns becoming targets for threat actors.
* **Implementation:** `User Properties` ➔ `Account` ➔ `Account Expires`
* **Status:** `[ ACTIVE ]` — Ensures ISO 27001 compliance. System automatically revokes all access rights at midnight on the specified contract end date, eliminating human error.

#### 🚨 Policy #04: Emergency Offboarding & Incident Response
* **Threat Vector:** Active internal threat, compromised account, or immediate employee termination.
* **Implementation:** Right Click ➔ `Disable Account`
* **Technical Mechanism:** Toggles the `ACCOUNTDISABLE` flag within the `userAccountControl` attribute.
* **Visual Indicator:** ⬇️ *A black downward arrow overlay appears on the user object.*
* **Status:** `[ TESTED & VERIFIED ]` — Account is instantly stripped of all authentication privileges network-wide.

---

### 🛠️ III. Routine L1/L2 Maintenance Logs
> **Note:** Successfully simulated and performed routine Helpdesk incident response procedures.
- `[✓]` Identified and unlocked accounts triggered by the brute-force `Account Lockout` security policy.
- `[✓]` Executed secure administrative password resets for simulated compromised credentials.

---
