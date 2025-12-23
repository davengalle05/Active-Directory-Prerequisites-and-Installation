# Active Directory – Prerequisites and Installation

This tutorial outlines the prerequisites and installation of a **Windows Server 2019 Active Directory Domain Services (AD DS)** environment within a virtualized internal network. This phase focuses on preparing the operating system, configuring networking, and promoting the server to a Domain Controller.

---

## Video Demonstration
*(Optional – add if recorded later)*  
YouTube: **Active Directory Tutorial (Phase 1/3) – Installation**

---

## Environments and Technologies Used

- VirtualBox (Virtual Machines / Compute)
- pfSense Firewall
- Active Directory Domain Services (AD DS)
- DNS
- Remote Desktop
- PowerShell

---

## Operating Systems Used

- Windows Server 2019  
- pfSense

---

## List of Prerequisites

- **Windows Server 2019 ISO:** Operating system used to deploy the domain controller.
- **VirtualBox:** Hypervisor used to host the Windows Server virtual machine.
- **pfSense:** Provides internal network routing and gateway services.
- **Static IP Addressing:** Required for reliable DNS and Active Directory functionality.
- **Administrator Privileges:** Required to install roles and promote the server.

---

## OVERVIEW

1. Prepare Windows Server 2019 for Active Directory services.
2. Configure internal networking and static IP addressing.
3. Install Active Directory Domain Services and DNS.
4. Promote the server to a Domain Controller.
5. Verify domain and DNS functionality.

---

## 1. Prepare Windows Server 2019 for Domain Services

Install Windows Server 2019 in a virtualized environment and configure it for internal network use.

- Allocate **4096 MB RAM** in VirtualBox.
- Set Network Adapter to **Internal Network (lan2)**.
- Disable unnecessary boot devices (e.g., floppy).
- Install VirtualBox Guest Additions.
- Remove installation media after setup.
- Rename the system to **DC1**.

---

## 2. Configure Network Settings

Assign a static IPv4 configuration to the server to ensure proper domain and DNS operation.

- IP Address: `10.80.80.2`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `10.80.80.1`
- Preferred DNS Server: `10.80.80.2`

---

## 3. Install Active Directory Domain Services and DNS

Install the required roles using Server Manager.

- Open **Server Manager → Manage → Add Roles and Features**.
- Select **Active Directory Domain Services (AD DS)**.
- Add required features.
- Select and install **DNS Server**.
- Complete role installation.

---

## 4. Promote Server to Domain Controller

Promote the server to a Domain Controller and create a new forest.

- Choose **Add a new forest**.
- Root Domain Name: `AD.Lab`
- NetBIOS Name: `AD`
- Configure Directory Services Restore Mode (DSRM) password.
- Complete prerequisite checks.
- Install and reboot the system.

---

## 5. Verification

Confirm that the installation was successful.

- Log in using domain credentials.
- Verify **Active Directory Users and Computers** opens correctly.
- Confirm **DNS Manager** is functioning.
- Ensure the system is operating as a Domain Controller.

---

## Conclusion

This phase establishes the foundation of the Active Directory environment by installing Windows Server 2019, configuring networking, and deploying Active Directory Domain Services. Subsequent phases will build on this foundation by configuring services, managing users, and applying Group Policy.
