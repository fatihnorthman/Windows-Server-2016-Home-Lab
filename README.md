<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=2b2d42,8d99ae,ef233c&height=220&section=header&text=Windows%20Server%202016%20Lab&fontSize=50&fontColor=ffffff&animation=fadeIn&fontAlignY=35" width="100%"/>
</div>

<p align="center">
  <code>Infrastructure</code> > <code>Windows Server</code> > <code>Identity Management & Client Integration</code>
</p>

# 🏛️ Windows Server 2016 Home Lab - Phase 3: ADUC & Client Provisioning

This phase focuses on the logical structuring of the Active Directory environment and the provisioning of the first Client OS. It covers Organizational Unit (OU) design, Security Group management, and overcoming modern OS deployment restrictions.

## 🚀 Phase Objectives
* **Client OS:** Windows 10 Pro (VMware Virtual Machine)
* **AD Management Tool:** Active Directory Users and Computers (ADUC)
* **Architecture:** Multi-branch logical structure (Sivas & Gaziantep)

---

## 🛠️ Step-by-Step Implementation

### 1. Logical Architecture Design (ADUC)
Designed a scalable, location-based Organizational Unit (OU) hierarchy to facilitate granular Group Policy Object (GPO) targeting in the future.
* **Root Domain:** `northman.com`
* **Branch 1 (Sivas):** Created parent OU `Sivas` with nested OUs: `Muhasebe` (Accounting), `IT`, and `Satis` (Sales).
* **Branch 2 (Gaziantep):** Created parent OU `Gaziantep` with nested OUs: `PC` and `Kullanicilar` (Users).

### 2. Identity & Access Management
* **User Provisioning:** Created multiple standard user accounts and structurally organized them by moving them into their respective departmental OUs.
* **Security Groups:** Established a Security Group named `Satis` within the `Sivas\Satis` OU. Added appropriate user accounts to this group to manage collective permissions.
* **Advanced Concepts:** Validated Group Nesting capabilities (adding groups as members of other groups) and mastered the removal of protected OUs by modifying the "Protect object from accidental deletion" attribute.

### 3. Client OS Provisioning
* **Deployment:** Installed Windows 10 as a secondary virtual machine to act as the primary domain client.
* **Optimization:** Installed VMware Tools on the client machine to ensure optimal network and display driver performance.

---

## ⚠️ Troubleshooting Log

### **Case #4: Windows 10 OOBE Local Account Enforcement**
* **Problem:** During the Windows 10 installation (Out-of-Box Experience), the setup aggressively demanded a Microsoft Account, hiding the "Offline Account" option.
* **Attempts:** 1. Opened CMD (`Shift+F10`) and attempted the `OOBE\BYPASSNRO` registry script (Failed - not applicable to this specific build).
  2. Attempted to kill the "Network Connection Flow" process via `taskmgr` (Failed - OS enforcement persisted).
* **Solution:** Performed a hardware-level intervention by virtually disconnecting the Network Adapter in VMware settings (simulating a physical ethernet cable unplug). This forced the OS setup to fallback to the Local Administrator account creation screen.

### **Case #5: Keyboard Layout Mismatch & AD Password Reset**
* **Problem:** Post-AD promotion, login attempts as the Domain Administrator failed continuously due to a hidden keyboard layout shift (TR/EN mismatch) during password creation.
* **Solution:** Utilized the On-Screen Keyboard via Accessibility Tools to bypass the physical layout mismatch and gain entry. Subsequently used the `dsa.msc` (ADUC) console to securely reset the Domain Administrator password without requiring the previous complex string.

---

## 📈 Current Progress
- [x] VMware Environment Optimization
- [x] Static IP & Hostname Configuration
- [x] Active Directory Domain Services (AD DS) Installation
- [x] DNS & Global Catalog Integration
- [x] **Client VM Provisioning (Win10 OOBE Bypass)**
- [x] **ADUC Hierarchy Design (OUs: Sivas, Gaziantep)**
- [x] **User and Security Group Management**
- [ ] **Next Step:** Joining the Windows 10 Client to the `northman.com` Domain
- [ ] **Next Step:** Group Policy Objects (GPO) Implementation
- [ ] **Next Step:** DHCP Server Role Configuration

---

<div align="center">
<p><i>"A well-structured Active Directory is like a well-organized library; everything has its place, and access is explicitly controlled."</i></p>
</div>
