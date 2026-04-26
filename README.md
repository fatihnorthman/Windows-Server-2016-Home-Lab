## 📦 Phase 12: Hybrid Infrastructure, DHCP Orchestration & RDP Service Hardening

> *"True infrastructure resilience is tested at the intersection of virtual control and physical hardware. A Junior System Administrator must not only deploy services but also master the underlying protocols and service life-cycles that govern connectivity."*

**Executive Summary:** This phase documents the expansion of the NORTHMAN lab environment into a hybrid physical-virtual state. Key achievements include the integration of physical laptop hardware into the Active Directory domain, the deployment of a centralized DHCP role to automate network provisioning, and the engineering of a domain-wide GPO to secure administrative RDP access. The phase concludes with a detailed Root Cause Analysis (RCA) regarding service-level initialization constraints and registry-level troubleshooting.

---

### I. Physical-Virtual Hybrid Migration & Hardware Integration
**Objective:** Eliminate "laboratory bias" by introducing physical hardware variables such as real NIC drivers, physical cabling, and external network latency into the domain environment.

* **Implementation Details:**
    * **Hardware Transition:** A dedicated physical laptop was prepared with a clean Windows 10 installation, moving away from a strictly nested virtual environment.
    * **Network Bridging:** The device was interfaced with the virtualized Domain Controller (Server1) through a bridged network configuration to allow Layer 2 broadcast communication and direct hardware-to-server interaction.
    * **Domain Join:** The laptop was successfully joined to the northman.com domain and strategically placed within the Sivas Organizational Unit (OU) for targeted policy application.
* **Outcome:** This setup confirms that the Active Directory infrastructure is robust enough to manage diverse endpoints across physical network boundaries, ensuring that policies and services propagate correctly to non-virtualized hardware.

---

### II. Network Services: Centralized DHCP Role Deployment
**Objective:** Transition from static IP management to a dynamic, scalable architecture using the DORA (Discover, Offer, Request, Acknowledge) protocol.

* **Role Configuration:**
    * **Authorization:** The DHCP role was installed and explicitly authorized in Active Directory to prevent "Rogue DHCP" servers from disrupting the network segment.
    * **Scope Engineering:** A new scope was created with a defined IP pool, excluding critical static addresses such as Domain Controllers and Gateways to prevent address conflicts.
* **Advanced Scope Options:**
    * **Option 003 (Router):** Defined the default gateway for the hybrid network.
    * **Option 006 (DNS Servers):** Pointed directly to the Domain Controller, ensuring that physical clients can resolve internal hostnames immediately upon lease acquisition.
* **Verification:** The physical laptop, configured to "Obtain an IP address automatically," successfully acquired a lease. This confirms the functional health of the DHCP broadcast traffic between the virtual server and physical endpoint.

---

### III. Administrative Orchestration: GPO-Driven RDP Deployment
**Objective:** Achieve a "Zero-Touch" administrative model where all endpoints are remotely accessible without manual local configuration, while maintaining strict firewall boundaries.

* **A. Service Configuration (Protocol Level):**
    * **Path:** Computer Configuration | Policies | Administrative Templates | Windows Components | Remote Desktop Services | Remote Desktop Session Host | Connections
    * **Setting:** "Allow users to connect remotely by using Remote Desktop Services" set to Enabled.
    * **Technical Impact:** This policy modifies the fDenyTSConnections registry key (setting the value to 0) on the target machines.
* **B. Network Security (Firewall Orchestration):**
    * **Path:** Computer Configuration | Policies | Windows Settings | Security Settings | Windows Defender Firewall with Advanced Security
    * **Implementation:** Created a Predefined Inbound Rule for Remote Desktop (TCP Port 3389).
    * **Security Rationale:** The rule was strictly constrained to the Domain Profile. This ensures that if the laptop is taken off-site (e.g., to a public Wi-Fi), the RDP port is automatically shielded, mitigating the risk of brute-force attacks from untrusted networks.

---

### IV. Troubleshooting & Root Cause Analysis (RCA)
**Objective:** Document the systematic resolution of a connectivity failure during the RDP rollout using standard systems engineering methodologies.

* **Incident Description:** Following the GPO link and a successful gpupdate /force on the physical client, RDP connections remained blocked. Initial suspicion fell on GPO propagation or OU hierarchy errors.
* **Diagnostic Methodology:**
    1. **Connectivity Test:** A ping to the hostname was successful, ruling out basic network isolation or DNS failure.
    2. **Policy Audit:** Executed "gpresult /r /SCOPE COMPUTER" as Administrator on the target laptop.
       - **Result:** The "RDP" GPO was listed under "Applied Group Policy Objects." This confirmed the GPO was correctly linked to the Sivas OU and the computer object had the necessary Read permissions from SYSVOL.
    3. **Port Analysis:** A "Test-NetConnection" on port 3389 returned False, indicating the port was not in a "listening" state despite the policy application.
* **Root Cause:** The Windows Remote Desktop Service (TermService) is a critical system component that reads its registry configuration (modified by GPO) during its initial startup phase. In this instance, the service did not dynamically refresh its listener state or re-bind to the port despite the registry values being updated in the background by the Group Policy engine.
* **Resolution:** A full system Reboot was performed. This forced the TermService to re-initialize, read the updated registry parameters, and successfully open the TCP 3389 listener port.

---

### V. Post-Implementation Validation
* **DNS-Based Routing:** Verified that RDP sessions could be initiated using the Hostname instead of the IP, confirming that the DHCP-DNS dynamic update registration is functioning as intended.
* **Role-Based Access:** Confirmed that only members of the Domain Admins group (e.g., NORTHMAN\mfşahan) could establish sessions, maintaining the Principle of Least Privilege (PoLP) and ensuring that unauthorized local users remain restricted from remote access.
