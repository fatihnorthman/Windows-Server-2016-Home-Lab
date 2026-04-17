<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=2b2d42,8d99ae,ef233c&height=220&section=header&text=Windows%20Server%202016%20Lab&fontSize=50&fontColor=ffffff&animation=fadeIn&fontAlignY=35" width="100%"/>
</div>

<p align="center">
  <code>Infrastructure</code> > <code>Windows Server</code> > <code>Active Directory Domain Services</code>
</p>

# 🏛️ Windows Server 2016 Home Lab - Phase 2: AD DS Promotion

This phase documents the transformation of a standalone Windows Server into the Root Domain Controller of a new Active Directory Forest. It details the forest architecture, functional levels, and critical directory services implementation.

## 🚀 Domain Architecture
* **Forest/Domain Name:** `northman.com`
* **NetBIOS Name:** `NORTHMAN`
* **Functional Level:** Windows Server 2016 (Forest & Domain)
* **Roles Deployed:** AD DS, DNS Server, Global Catalog (GC)

---

## 🛠️ AD DS Promotion Details

### 1. Forest Configuration
- **New Forest Creation:** Established `northman.com` as the root of a new forest.
- **Functional Levels:** Set both Forest and Domain functional levels to **Windows Server 2016** to utilize modern security features like Privileged Access Management and rolling expiration for group memberships.
- **Global Catalog (GC):** Enabled GC to provide searchable cluster of all objects within the forest, essential for multi-domain queries and UPN logons.

### 2. Database & Replication Paths
The standard NTDS directory structure was maintained for optimal compatibility:
- **Database Folder:** `C:\Windows\NTDS`
- **Log Files:** `C:\Windows\NTDS`
- **SYSVOL Folder:** `C:\Windows\SYSVOL` (Used for Group Policy and logon script replication).

---

## ⚠️ Phase 2 Troubleshooting Log

### **Case #3: DNS Verification Latency during Promotion**
* **Problem:** During the "Deployment Configuration" stage, the wizard became unresponsive for several minutes after entering the Root Domain Name.
* **Root Cause:** The server was attempting to query external DNS resolvers (e.g., `8.8.8.8`) to check for the existence of the `northman.com` namespace. This caused a significant timeout period before failing over to local validation.
* **Analysis:** High latency in AD promotion is often tied to recursive DNS lookups for non-existent local domains.
* **Solution:** Patience was exercised until the timeout occurred, allowing the wizard to proceed with local forest creation. *Post-install optimization:* Ensured DNS Preferred address points to `127.0.0.1`.

---

## 💻 Identity & Access Management
Following the promotion, the server transitioned from local SAM (Security Accounts Manager) authentication to Domain-based authentication.
- **Logon Format:** `NORTHMAN\Administrator`
- **Transition:** Local accounts were migrated to the Active Directory database, and the server now manages centralized identity for the entire namespace.

---

## 📈 Current Progress
- [x] VMware Environment Optimization
- [x] Static IP & Hostname Configuration
- [x] Bidirectional Network Connectivity
- [x] **Active Directory Domain Services (AD DS) Installation**
- [x] **New Forest Creation (northman.com)**
- [x] **DNS & Global Catalog Integration**
- [ ] **Next Step:** Active Directory Users and Computers (ADUC) Management
- [ ] **Next Step:** Group Policy Objects (GPO) Implementation
- [ ] **Next Step:** DHCP Server Role Configuration

---

<div align="center">
<p><i>"The Forest has been planted. Now it's time to manage the trees."</i></p>
</div>
