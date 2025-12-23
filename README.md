# Active Directory – Prerequisites and Installation

This tutorial outlines the prerequisites and installation of a **Windows Server 2019 Active Directory (AD) domain environment** within a virtualized internal network.

---

## Video Demonstration
*(Optional – add later if recorded)*  
YouTube: **Active Directory Tutorial (Phase 1/?) – Installation**

---

## Environments and Technologies Used

- VirtualBox (Virtual Machines / Compute)
- pfSense Firewall
- Active Directory Domain Services (AD DS)
- DNS
- DHCP
- Group Policy Management
- PowerShell

---

## Operating Systems Used

- Windows Server 2019  
- pfSense

---

## List of Prerequisites

- **Windows Server 2019 ISO:** Operating system used to deploy the Active Directory Domain Controller.
- **VirtualBox:** Hypervisor used to host the Windows Server virtual machine.
- **pfSense:** Firewall/router used to provide internal network routing.
- **Static IP Configuration:** Required for domain controller and DNS reliability.
- **Administrator Access:** Required for role installation and configuration.

---

## OVERVIEW

1. Prepare the Windows Server 2019 OS for domain services.
2. Configure internal networking and static IP addressing.
3. Install Active Directory Domain Services and DNS.
4. Promote the server to a Domain Controller.
5. Configure DNS and DHCP for the domain.
6. Create domain users and assign permissions.
7. Apply Group Policy Objects (GPOs).
8. Prepare the environment for administrative access and testing.

---

## 1. Prepare Windows Server 2019 for Domain Services

Install Windows Server 2019 in VirtualBox and configure the system for internal network use.

- Allocate **4096 MB RAM**
- Set Network Adapter to **Internal Network (lan2)**
- Install VirtualBox Guest Additions
- Remove installation media after setup
- Rename system to **DC1**

---

## 2. Configure Network Settings

Assign a static IP address to the server.

- IP Address: `10.80.80.2`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `10.80.80.1`
- Preferred DNS Server: `10.80.80.2`

This ensures proper DNS and domain functionality.

---

## 3. Install Active Directory and DNS

Install Active Directory Domain Services and DNS using Server Manager.

- Open **Server Manager → Add Roles and Features**
- Select **Active Directory Domain Services**
- Add required features and DNS Server
- Promote the server to a Domain Controller:
  - New Forest: `AD.Lab`
  - NetBIOS Name: `AD`
- Complete installation and reboot

---

## 4. Configure DNS and DHCP

### DNS Configuration
- Configure DNS forwarder:
  - Forwarder IP: `10.80.80.1`

### DHCP Configuration
- Install DHCP Server role
- Create a DHCP scope:
  - Start IP: `10.80.80.11`
  - End IP: `10.80.80.25`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `10.80.80.1`
- Activate DHCP scope

---

## 5. Create Domain Users and Assign Permissions

Create domain users using **Active Directory Users and Computers**.

- **MaraCyber** – Domain Administrator  
- **KarmaCyber** – Standard / Exploitable User  
- **KLCyber** – Standard User  

Add **MaraCyber** to the **Domain Admins** group.

---

## 6. Enable Administrative and Remote Features

Configure Group Policy Objects (GPOs) to allow administration and testing.

- Enable Remote Desktop Protocol (RDP)
- Enable Windows Remote Management (WinRM)
- Enable Remote Procedure Call (RPC)
- Configure registry settings for remote administrative access
- Apply policies using `gpupdate /force`

---

## 7. Prepare Vulnerable Lab Environment (Testing Only)

For learning and testing purposes:

- Modify PowerShell execution policy
- Execute scripts to intentionally weaken Active Directory security
- Disable Defender and Firewall via Group Policy (lab use only)

---

## Conclusion

This lab demonstrates the deployment and configuration of a **Windows Server 2019 Active Directory environment**, closely mirroring real-world enterprise domain setups. The project builds foundational IT Support and Systems Administration skills while enabling future security testing and learning scenarios.
