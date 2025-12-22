# Windows Server 2019 – Active Directory Domain Setup

This tutorial outlines the step-by-step installation, configuration, and security hardening of a Windows Server 2019 Active Directory (AD) environment in a virtualized lab. The guide is tailored for IT Support and Systems Administration learning and features realistic Domain deployment, DNS/DHCP, GPO, and vulnerability preparations.

---

## Environments and Technologies Used

- VirtualBox (Virtualization)
- Windows Server 2019 (Evaluation)
- pfSense Firewall (Virtual Router)
- Active Directory Domain Services (AD DS)
- DNS/DHCP Services
- Group Policy Management
- PowerShell

## Operating Systems Used

- Windows Server 2019
- pfSense

---

## Prerequisites

- Download [Windows Server 2019 Evaluation ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019).
  - Download the 64-bit ISO and rename it to “windows server 2019”.
- VirtualBox installed with at least 4096MB memory allocated for the VM.
- pfSense virtual appliance for network segmentation.

---

## OVERVIEW

1. Prepare and install Windows Server 2019 in VirtualBox.
2. Set up internal networking and pfSense for isolation.
3. Configure static IP and DNS on the server for lab segmentation.
4. Install and configure Active Directory Domain Services (AD DS).
5. Set up DNS and DHCP for AD lab environment.
6. Create test users with varying permissions.
7. Harden environment, apply GPOs, and enable remote services (RDP, WinRM, RPC).
8. Prepare an intentionally vulnerable AD environment for security testing.

---

## Step-by-Step Guide

### 1. Download and Install Windows Server 2019

- Download from [Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019).
- Add ISO to VirtualBox:
  - Assign 4096MB memory
  - Set network to “Internal Network” (lan2)
  - Disable floppy, place hard drive and optical at boot priority
- Install Windows Server 2019 and Guest Additions (run VBoxGuestAdditions executable, reboot as needed).
- Remove optical drive after installation.

### 2. Network Setup (Internal Network via pfSense)

- Ensure server is on an “Internal Network” (no WiFi/internet, isolated lab).
- Set static IP:
  - Open Network & Internet settings → Adapter → Properties → IPv4 Settings.
  - Set the following:
    - IP Address: 10.80.80.2
    - Subnet Mask: 255.255.255.0
    - Default Gateway: 10.80.80.1
    - Preferred DNS: 10.80.80.2
  - Rename PC to “DC1”.

### 3. Install and Configure Active Directory Domain Services

- Open Server Manager → Manage → Add Roles and Features.
  - Choose role-based installation, select DC1.
  - Enable “Active Directory Domain Services” and “DNS Server”; add required features.
  - Install and promote server to a domain controller:
    - New forest: Root domain = `AD.Lab`
    - Directory restore password: `Dave2005`
    - NetBIOS Name: `AD`
- Complete promotion and installation.

### 4. DNS and DHCP Configuration

- Set up DNS forwarder to pfSense (10.80.80.1):
  - Open DNS Manager → Forwarders → Edit → Add 10.80.80.1.
- Add DHCP Server:
  - Server Roles → Add DHCP server role.
  - Configure DHCP scope in DHCP Manager:
    - Start IP: 10.80.80.11 | End IP: 10.80.80.25 | Mask: 255.255.255.0
    - Default Gateway: 10.80.80.1
    - Activate scope.

### 5. Create Users and Assign Permissions

- Use “Active Directory Users and Computers” to create users:

| First Name | Last Name | Username     | Password     | Permission Level  |
|------------|-----------|--------------|--------------|-------------------|
| Mara       | Cyber     | MaraCyber    | Dave2005     | Domain Admin      |
| Karma      | Cyber     | KarmaCyber   | Dave@2005    | Exploitable User  |
| KL         | Cyber     | KLCyber      | Dave@@2005   | Standard User     |

- Assign MaraCyber to Domain Admins.

### 6. Security Hardening & Vulnerability Lab Setup

- Disable password expiration for test users as needed.
- Open PowerShell (as Admin) and execute:
  ```powershell
  Set-ExecutionPolicy -ExecutionPolicy Bypass -Force
  [System.Net.WebClient]::new().DownloadString('https://raw.githubusercontent.com/WaterExecution/vulnerable-AD-plus/master/vulnadplus.ps1') -replace 'change\.me','ad.lab' | Invoke-Expression
  ```
- Restart as needed.

### 7. Group Policy: Disable Defender, Firewall, and Enable Remote Features

- In Group Policy Management:
  - Create GPO “Disable Protection”:
    - Disable Windows Defender & Firewall (see detailed steps above).
  - Create GPO “Admin Remote Login”:
    - Registry entry: `LocalAccountFilterPolicy = 1` (REG_DWORD).
  - Enable WinRM, RDP, and RPC via dedicated GPOs, following outlined steps above for each corresponding role.

---

## Testing and Access

- Login as created users to verify permissions.
- Test RDP/WinRM/RPC connections.
- Validate DHCP functionality across the internal network.
- Confirm group policy changes are effective.

---

## Additional Resources

- [Explanation of Lab on Notion](https://www.notion.so/EXPLANATION-OF-LAB-2a467d4fe6888062bacadd0905926d34?pvs=21)

---

## Screenshots

*Insert key screenshots here to help visualize steps and verify configurations*
