## 🛡️ Phase 8: Group Policy Orchestration & Endpoint Lockdown

> *"Group Policy is the nervous system of an enterprise domain. It translates security intent into absolute enforcement across the infrastructure."*

**Executive Summary:** This phase documents the deployment of centralized Group Policy Objects (GPOs) to enforce security baselines, provision resources, and manipulate policy inheritance. Critical operations included establishing automated network drive mappings, executing endpoint lockdowns, and successfully engineering a firewall exception to allow remote GPO push commands.

---

### 🏗️ I. GPO Lifecycle & Deployment Architecture
**Objective:** Standardize the creation and linking methodologies for Group Policy Objects.

* **Deployment Methodology:** Validated two primary deployment vectors:
  1. **Centralized Creation:** Creating the GPO in the `Group Policy Objects` container, then linking it strategically to target Organizational Units (OUs).
  2. **Direct Linking:** Right-clicking the target OU and selecting "Create a GPO in this domain, and Link it here..." for rapid deployment.
* **Editing & Targeting:** Utilized the Group Policy Management Editor to modify registry-based administrative templates and user-specific configurations.

---

### 🔒 II. Endpoint Security & User Lockdown
**Objective:** Restrict standard user capabilities to prevent system tampering and data exfiltration.

| Policy Name | GPO Path / Mechanism | Technical Outcome |
| :--- | :--- | :--- |
| **Hide OS Drives** | `User Config ➔ Admin Templates ➔ Windows Components ➔ File Explorer` | Masked the `C:` drive from standard users, preventing accidental modification of OS directories while maintaining background system access. |
| **Disable Power Options** | `User Config ➔ Admin Templates ➔ Start Menu and Taskbar` | Removed "Shutdown" and "Restart" options from the Start Menu, ensuring endpoints remain online for automated, after-hours patching. |
| **Drive Mapping (GPP)** | `User Config ➔ Preferences ➔ Windows Settings ➔ Drive Maps` | Automated the mounting of corporate network shares upon user logon, replacing manual client-side configuration. |

---

### 🚦 III. Policy Hierarchy & Conflict Resolution
**Objective:** Master the flow of policy inheritance and mandate critical security baselines across departmental sub-OUs.

1. **Inheritance Flow:** Verified that policies linked at a parent OU naturally cascade down to all child OUs.
2. **Block Inheritance:** Successfully isolated specific sub-OUs (e.g., an IT Test Lab) by enabling `Block Inheritance`. This severed the flow of standard corporate policies from the parent container.
3. **The "Enforced" Override:** Demonstrated absolute policy compliance. By setting a critical GPO to **`Enforced`** at the parent level, the policy successfully bypassed the `Block Inheritance` flag on the child OU, proving that "Enforced always wins."

---

### 📡 IV. The "Chicken-and-Egg" Firewall Exception
**Objective:** Enable the Domain Controller to remotely push `gpupdate /force` commands without native Windows Defender Firewalls blocking the RPC/WMI traffic.

* **Engineering the Exception:** Created a dedicated GPO (`GPO_Firewall_RemoteUpdate`) with inbound firewall rules permitting **WMI** and **Remote Scheduled Tasks Management**.
* **Bootstrapping the Client:** Overcame the logical paradox (needing the policy to push the policy) by executing one final, manual `gpupdate /force` locally on the Windows 10 endpoint. 
* **Outcome:** The client successfully digested the firewall exception. All subsequent GPO updates are now pushed seamlessly from the Server (GPMC) without error `8007071a`.
