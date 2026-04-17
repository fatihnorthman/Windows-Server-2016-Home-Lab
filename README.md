<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=2b2d42,8d99ae,ef233c&height=220&section=header&text=Windows%20Server%202016%20Lab&fontSize=50&fontColor=ffffff&animation=fadeIn&fontAlignY=35" width="100%"/>
</div>

<p align="center">
  <code>Infrastructure</code> > <code>Windows Server</code> > <code>Network & Security Hardening</code>
</p>

# 🏛️ Windows Server 2016 Home Lab - Phase 1: Environment & Connectivity

This repository documents the comprehensive setup of a Windows Server 2016 environment. It details the initial deployment, advanced network troubleshooting, and security configuration required to prepare the server for an Active Directory (AD DS) role.

## 🚀 Project Overview
* **Hypervisor:** VMware Workstation
* **OS:** Windows Server 2016 Datacenter Evaluation
* **Network Mode:** Bridged (Direct physical network access)
* **Static IP:** `192.168.1.200`
* **Status:** Fully optimized and network-ready.

---

## 🛠️ Step-by-Step Implementation

### 1. Provisioning & Optimization
- **Bypassing Easy Install:** Resolved VMware automation issues by manually mounting the ISO, ensuring access to the Evaluation Edition setup.
- **VMware Tools:** Integrated essential drivers for display resolution, network performance, and guest-host interaction.
- **Server Identity:** Renamed the server and assigned a static IP address to ensure reliability for future Domain Controller services.

### 2. Connectivity & Security Adjustments
- **IE Enhanced Security Configuration (IE ESC):** Disabled IE ESC for the Administrator account to facilitate necessary browser-based downloads and laboratory research.
- **DNS Loopback Management:** Configured `127.0.0.1` as the primary DNS in preparation for the DNS Role, with a public recursive resolver as secondary to maintain internet access.

---

## ⚠️ Troubleshooting Log (Vulnerability & Connectivity Analysis)

### **Case #1: Bidirectional ICMP (Ping) Failures**
* **Problem:** VM-to-Host and Host-to-VM ping requests resulted in `Request timed out`.
* **Root Cause:** Windows Defender Firewall blocks incoming ICMPv4 (Echo Request) packets by default on both Client and Server OS.
* **Solution:** 1. On the **Host Machine**: Enabled the inbound rule **"File and Printer Sharing (Echo Request - ICMPv4-In)"** for Private/Public profiles.
    2. On the **Server Machine**: Applied the same rule within the Server's Firewall settings to allow the Host to reach the Guest.

### **Case #2: The DNS Resolution Paradox**
* **Problem:** Could ping public IPs (e.g., `8.8.8.8`) but domain resolution (e.g., `google.com`) failed.
* **Root Cause:** The Preferred DNS was set to `127.0.0.1` (Localhost) because the server is destined to be a Domain Controller. However, since the **DNS Server Role** was not yet installed, the server had no local service to resolve queries.
* **Solution:** Added a reliable Public DNS (`8.8.8.8`) as the **Alternate DNS Server**. This allowed the OS to failover to a public resolver while maintaining the primary configuration for the upcoming AD DS deployment.

---

## 📈 Current Progress
- [x] VMware Environment Optimization
- [x] Static IP & Hostname Configuration
- [x] Bidirectional Network Connectivity (Firewall Hardening)
- [x] DNS Resolver Failover Configuration
- [x] IE ESC Disabled for Administrative Ease
- [ ] **Next Step:** Installation of Active Directory Domain Services (AD DS)
- [ ] **Next Step:** DNS Zone Configuration

---

<div align="center">
<p><i>"A Sysadmin doesn't just build systems; they solve the puzzles that keep those systems talking to each other."</i></p>
</div>
