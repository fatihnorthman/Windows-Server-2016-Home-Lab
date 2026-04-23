## 🗄️ Phase 7: Storage Governance & Data Loss Prevention (FSRM)

> *"Storage space is inexpensive, but unmanaged data sprawl is a critical liability. Effective governance ensures both availability and security."*

**Executive Summary:** This phase transitions the infrastructure from basic file sharing to advanced storage governance. By deploying the **File Server Resource Manager (FSRM)** role, we established automated capacity limits (Quotas), implemented Data Loss Prevention via File Screening, and integrated an SMTP relay for proactive administrative alerting.

---

### 📏 I. Capacity Management & Custom Quotas
**Objective:** Prevent storage exhaustion on the File Server by enforcing user and department-level storage limits.

* **Drive Mapping Validation:** Successfully deployed network drives to end-user workstations to streamline access to the central repository.
* **Custom Template Creation:** Evaluated default FSRM templates (e.g., 2GB limit) and found them insufficient for the simulated corporate scenario. Navigated to `Tools ➔ FSRM ➔ Quota Management` and engineered a custom **8GB Quota Template**.
* **Quota Architecture:**
    * **Hard Quota:** Configured absolute limits that physically block users from saving data once the 8GB threshold is reached, ensuring server stability.
    * **Soft Quota:** Utilized for monitoring purposes, allowing users to exceed the limit while generating compliance alerts.

---

### 📧 II. Proactive Alerting & Event Automation
**Objective:** Establish a notification pipeline to alert the IT department *before* a critical storage failure occurs.

* **SMTP Relay Configuration:** Configured the FSRM global options (`Right-Click FSRM ➔ Configure Options`) to integrate a simulated Mail Server via IP/Hostname, defining the central Admin email address for incoming alerts.
* **Threshold Triggers:** Modified the custom Quota Template to include progressive **Notification Thresholds**. 
    * *Example Protocol:* At **85% capacity**, an automated warning email is generated and dispatched to the IT team.
* **Alert Customization:** Customized the dynamic email templates to include precise variables `[Source IoC, User, Capacity]` and configured secondary triggers to write directly to the Windows Event Log.

---

### 🛑 III. File Screening & Threat Mitigation
**Objective:** Protect corporate storage assets from unauthorized, non-business, or potentially malicious file types.

| Feature | Implementation | Technical Outcome |
| :--- | :--- | :--- |
| **File Screening** | `FSRM ➔ File Screening Management` | Blocked specific extensions (e.g., `.exe`, `.mp3`, `.mp4`) from being written to corporate shares. |
| **Active Auditing** | FSRM Event Triggers | If a user attempts to upload a restricted file type, the system instantly drops the payload and flags the action. |
| **Log Aggregation** | `Event Viewer ➔ Windows Logs` | Traced and analyzed FSRM security events to identify which users attempted to bypass the File Screen policies. |

---

### ✅ IV. Deployment & Validation Checklist
- `[✓]` **Network Drives:** Users can access the File Server seamlessly via mapped drives on their Windows 10 endpoints.
- `[✓]` **Custom Quotas:** 8GB Hard Quota successfully applied to target directories.
- `[✓]` **Email Integration:** SMTP routing configured for automated capacity warnings.
- `[✓]` **Screening Active:** Unauthorized file types are actively blocked and logged to the Event Viewer.
