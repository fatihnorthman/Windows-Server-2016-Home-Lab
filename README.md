---

## 📂 Phase 5: File Server Infrastructure & Granular Access Control
*“Data is the lifeblood of an organization; its accessibility must be seamless, but its security must be absolute.”*

This phase documents the provisioning of a dedicated storage environment and the implementation of a hierarchical permission model. By deploying a second server specifically for file services, we have moved from a single-point infrastructure to a scalable, distributed network architecture.

### 🏗️ I. Server Provisioning & Domain Integration
**Objective:** Deploy a specialized node for centralized data storage and integrate it into the `northman.com` security boundary.

* **Node Identity:** Provisioned a new Virtual Machine running **Windows Server 2016**, designated as `File-Storage`.
* **Networking & Connectivity:** * Assigned a static IP within the server VLAN and configured the DNS to point to the primary Domain Controller (`192.168.1.200`).
    * **Firewall Hardening:** Modified Inbound Rules to enable **ICMPv4 (Echo Request)**, allowing diagnostic `ping` tests to verify network path integrity between the DC, File Server, and Client endpoints.
* **Domain Handshake:** Successfully joined `File-Storage` to the `northman.com` domain, allowing the server to recognize and authenticate domain-wide security principals (Users and Groups).

---

### 🛡️ II. Storage Architecture & Permission Modeling
**Objective:** Implement a "Least Privilege" access model by segregating Share-level and NTFS-level permissions.

| Operation Phase | Technical Implementation | Outcome |
| :--- | :--- | :--- |
| **Share Creation** | SMB (Server Message Block) Protocol | Created a centralized repository and enabled "Sharing" to make it discoverable over the network. |
| **Share Permissions** | `Properties` ➔ `Sharing` ➔ `Permissions` | Defined the entry-point access. Set as a broad "Change" gate for authenticated users to be refined by NTFS. |
| **NTFS Hardening** | `Properties` ➔ `Security` (Advanced) | Applied granular Access Control Entries (ACEs). Configured Read, Write, List, and Execute rights for specific departments. |

#### 📝 Field Observation: The "Effective Permissions" Logic
During validation, the interaction between Share and NTFS permissions was tested. 
- **Scenario:** A non-admin user accessed the share from a Windows 10 terminal.
- **Result:** While the share was visible to all authenticated users, the **NTFS Security Layer** acted as the ultimate gatekeeper, preventing unauthorized users from modifying or deleting files within restricted departmental folders.

---

### 🛠️ III. Deployment Checklist & Validation
> **Note:** Verified the environment using standard L1/L2 testing procedures.

- `[✓]` **DNS Resolution:** Verified that `File-Storage` resolves correctly across all subnets.
- `[✓]` **Access Validation:** Successfully mapped network drives on Windows 10 clients using standard user accounts.
- `[✓]` **Permission Audit:** Confirmed that "List Folder Contents" works for unauthorized users, but "Write/Delete" actions are strictly denied as per the NTFS ACL (Access Control List).
- `[✓]` **ICMP Verification:** Network latency and packet loss between nodes remain within optimal parameters ( <1ms).

---
