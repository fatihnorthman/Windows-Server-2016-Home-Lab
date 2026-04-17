<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=2b2d42,8d99ae,ef233c&height=220&section=header&text=Windows%20Server%202016%20Lab&fontSize=50&fontColor=ffffff&animation=fadeIn&fontAlignY=35" width="100%"/>
</div>

<p align="center">
  <code>Infrastructure</code> > <code>Windows Server</code> > <code>Active Directory Preparation</code>
</p>

# 🏛️ Windows Server 2016 Home Lab - Phase 1: Environment & Network

This repository documents the first phase of building an enterprise-grade IT infrastructure from scratch. The project covers everything from setting up the virtualization layer to operating system optimization and critical network configurations.

## 🚀 Project Overview
* **Hypervisor:** VMware Workstation
* **OS:** Windows Server 2016 Datacenter Evaluation (Desktop Experience)
* **Goal:** To deploy a secure, scalable, and enterprise-standard Active Directory Domain environment.

---

## 🛠️ Setup & Configuration Steps

### 1. Hypervisor Layer & ISO Boot
The virtual machine architecture was created, overcoming hypervisor automation hurdles with manual interventions.

* **Virtual Hardware:** Resource allocation and hardware provisioning were completed on VMware.
* **Network Mode:** **Bridged Networking** was selected to allow the server to communicate directly with physical devices on the local network (same subnet as the host machine: `192.168.1.101`).

### 2. OS Optimization
* **Edition Selection:** Installed the *Datacenter Evaluation* edition for unrestricted virtualization features and full GUI support.
* **Driver Integration:** Successfully installed **VMware Tools** for optimal graphical performance, network stability, and seamless host-guest interaction.
* **Identity Management:** Configured the initial Local Administrator account adhering to security standards.

### 3. Network & Identity Configuration (Critical)
Before adding the Active Directory role, the server's network identity was strictly defined:

* **Hostname Configuration:** Changed the default random hostname to a standardized, professional server name via Server Manager.
* **Static IP Assignment:** * **IP Address:** `192.168.1.200` (Reserved on the bridged network)
    * **Subnet Mask:** `255.255.255.0`
    * **Default Gateway:** `192.168.1.1`
* **DNS Configuration:** Since this machine will act as the primary Domain Controller and DNS Server, its own IP address (`192.168.1.200`) was assigned as the Preferred DNS Server.

---

## ⚠️ Troubleshooting Log

### **Case #1: Bypassing VMware Easy Install Restriction**
* **Issue:** VMware's "Easy Install" feature automatically detected the Windows Server ISO and demanded a "Product Key" upfront, blocking the installation process for the Evaluation edition.
* **Analysis:** The hypervisor's automated setup prevents access to the standard Windows setup screen where the "I don't have a product key" option resides.
* **Solution:** Created the virtual machine initially using the *"I will install the operating system later"* option (creating a blank drive). Afterward, manually mounted the ISO file to the virtual CD/DVD drive in the hardware settings. This forced a manual boot, allowing a successful bypass and standard installation.

---

## 📈 Current Status & Next Steps
- [x] VMware Virtual Machine Configuration
- [x] Windows Server 2016 Installation
- [x] VMware Tools & Driver Optimization
- [x] Hostname and Static IP Configuration
- [ ] **Next Step:** Installation of Active Directory Domain Services (AD DS) Role
- [ ] **Next Step:** DNS and DHCP Role Configurations

---

<div align="center">
<p><i>"Network configuration is the foundation of a system; the stronger the foundation, the higher the uptime."</i></p>
</div>
