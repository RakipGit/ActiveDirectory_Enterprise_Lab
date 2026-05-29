![Status](https://img.shields.io/badge/status-complete-brightgreen)

# Active Directory Enterprise Lab: AD DS, NAT, DHCP, GPOs, RBAC File Shares and Security Auditing

A Windows Server lab that demonstrates the deployment and administration of an Active Directory environment using Hyper-V, Windows Server 2019, Windows 10, AD DS, DNS, DHCP, NAT/RRAS, Group Policy, RBAC file sharing and Windows security auditing.

---

## Project Summary

This project simulates a enterprise Windows domain environment built from scratch in Hyper-V. I configured a Windows Server 2019 machine as a Domain Controller, created an Active Directory domain, deployed DHCP and NAT/RRAS, joined a Windows 10 client to the domain, created users and security groups, configured shared folder permissions, applied Group Policy Objects, and enabled audit and event monitoring. The goal of this lab was to understand how core Microsoft infrastructure services work together in a centralized domain environment.

---

## Lab Architecture

The lab was built with **two virtual machines** in Hyper-V:

| Machine   | Operating System    | Role                                                                                    |
| --------- | ------------------- | --------------------------------------------------------------------------------------- |
| `RAKIPDC` | Windows Server 2019 | Domain Controller, DNS, DHCP, NAT/RRAS, File Server, GPO Management, Audit Policy       |
| `CLIENT1` | Windows 10 Pro      | Domain-joined workstation used for testing users, GPOs, file access, and authentication |

The Domain Controller was configured with two network adapters:

| Adapter          | Purpose                                                          |
| ---------------- | ---------------------------------------------------------------- |
| External Adapter | Provides internet connectivity                                   |
| Internal Adapter | Provides private domain network connectivity for client machines |

The internal network was configured so the Windows 10 client could receive an IP address from DHCP and reach the internet through NAT/RRAS on the Domain Controller.

---

## What I Did

### 1. Hyper-V and Virtual Machine Setup

* Created the lab environment using **Hyper-V**
* Downloaded Windows Server 2019 and Windows 10 ISOs
* Created a Windows Server 2019 virtual machine
* Installed Windows Server 2019 with **Desktop Experience**
* Created a Windows 10 Pro client virtual machine
* Connected the Windows 10 client to the internal Hyper-V switch

### 2. Domain Controller Deployment

* Renamed the Windows Server machine to `RAKIPDC`
* Configured a static IP address on the server
* Installed the **Active Directory Domain Services** role
* Promoted the server to a Domain Controller

### 3. NAT/RRAS and Internal Networking

* Added a second internal Hyper-V virtual switch network adapter to the Domain Controller
* Configured the internal adapter with a separate private subnet
* Installed and configured **Routing and Remote Access Service**
* Enabled NAT so the internal Windows 10 client could access the internet through the Domain Controller
* Verified connectivity from the client machine using cmd network tests

### 4. DHCP Configuration

* Installed the **DHCP Server** role
* Created a DHCP scope for the internal network
* Configured the Domain Controller’s internal IP as the default gateway for clients
* Activated the DHCP scope
* Verified that `CLIENT1` received an IP address automatically from the DHCP server scope
* Checked DHCP leases from the server to see that everything works fine

### 5. Client Domain Join

* Installed Windows 10 Pro on the client VM
* Verified that the client received DHCP configuration
* Verified connectivity to the domain
* Joined the Windows 10 client to the `rakip.com` domain
* Confirmed that the client computer object appeared in Active Directory

### 6. Active Directory User and Group Management

* Added an administrative user to the **Domain Admins** group
* Used a PowerShell script to bulk-create more than 1,000 users
* Created Organizational Units inside Active Directory

### 7. RBAC-Style File Sharing with Security Groups

* Created security groups for different access levels and departments
* Configured Windows file sharing
* Applied NTFS permissions to folders based on Active Directory security groups
* Tested access from the Windows 10 domain joined client
* Verified that users could only access the folders they were authorized to access by the departments they were part of
* Confirmed that unauthorized access attempts were blocked

### 8. Group Policy Configuration

* Created Group Policy Objects using Group Policy Management
* Configured a GPO to block access to **Control Panel** and **Settings** for selected users
* Configured a centralized wallpaper policy 
* Configured screen saver settings
* Enabled password protection for the screen saver
* Configured automatic screen lock after 5 minutes of inactivity
* Linked GPOs to OUs and groups
* Verified that the policies applied successfully on the client machine

### 9. Security Auditing and Event Monitoring

* Configured audit policies for security monitoring
* Monitored authentication and account-related events in **Event Viewer**
* Reviewed failed logon activity
* Reviewed user and account changes
* Reviewed group membership changes
* Used Event Viewer filtering to inspect security events

---

## Screenshots

![Active Directory Lab Overview](images/Screenshot (73).png)

<details>
<summary>🔎 View Full Lab Walkthrough (Screenshots)</summary>

### 1. Hyper-V VM Creation

![Hyper-V VM Creation](images/hyperv-vm-creation.png)

### 2. Windows Server 2019 Installation

![Windows Server Installation](images/windows-server-installation.png)

### 3. Server Renamed to RAKIPDC

![Rename Server](images/rename-server-rakipdc.png)

### 4. Static IP Configuration

![Static IP Configuration](images/static-ip-domain-controller.png)

### 5. Installing Active Directory Domain Services

![Install AD DS](images/install-ad-ds.png)

### 6. Promoting the Server to Domain Controller

![Promote Domain Controller](images/promote-domain-controller.png)

### 7. Creating the Domain rakip.com

![Create Domain](images/create-domain-rakip.png)

### 8. Active Directory Users and Computers

![ADUC](images/aduc-domain-ready.png)

### 9. Creating Organizational Units and Users

![OU Users](images/create-ou-users.png)

### 10. Adding User to Domain Admins

![Domain Admins](images/domain-admins-user.png)

### 11. Installing Routing and Remote Access

![Install RRAS](images/install-rras.png)

### 12. Internal Hyper-V Switch Creation

![Internal Switch](images/internal-hyperv-switch.png)

### 13. Adding Internal Adapter to the Domain Controller

![Internal Adapter](images/add-internal-adapter.png)

### 14. Configuring NAT/RRAS

![NAT RRAS](images/configure-nat-rras.png)

### 15. DHCP Server Installation

![Install DHCP](images/install-dhcp.png)

### 16. DHCP Scope Configuration

![DHCP Scope](images/dhcp-scope.png)

### 17. Windows 10 Client VM Setup

![Windows 10 Client](images/windows10-client-vm.png)

### 18. Client Receiving DHCP Address

![Client DHCP](images/client-dhcp-ip.png)

### 19. Internet Connectivity Test

![Ping Test](images/client-internet-ping.png)

### 20. Domain Connectivity Test

![Domain Ping](images/client-domain-ping.png)

### 21. Joining CLIENT1 to the Domain

![Join Domain](images/client-join-domain.png)

### 22. DHCP Lease Verification

![DHCP Lease](images/dhcp-client-lease.png)

### 23. Bulk User Creation with PowerShell

![Bulk User Creation](images/bulk-users-powershell.png)

### 24. Created Users in Active Directory

![Created Users](images/ad-created-users.png)

### 25. Security Groups for File Access

![Security Groups](images/security-groups-rbac.png)

### 26. Shared Department Folders

![Department Folders](images/shared-department-folders.png)

### 27. NTFS Permission Configuration

![NTFS Permissions](images/ntfs-folder-permissions.png)

### 28. Testing Authorized Folder Access

![Authorized Folder Access](images/authorized-folder-access.png)

### 29. Testing Access Denied for Unauthorized Folder

![Access Denied](images/access-denied-folder.png)

### 30. GPO: Control Panel and Settings Restriction

![Control Panel GPO](images/control-panel-settings-gpo.png)

### 31. GPO: Centralized Wallpaper

![Wallpaper GPO](images/centralized-wallpaper-gpo.png)

### 32. GPO: Screen Lock Policy

![Screen Lock GPO](images/screen-lock-gpo.png)

### 33. Audit Policy Configuration

![Audit Policy](images/audit-policy-configuration.png)

### 34. Event Viewer Security Logs

![Event Viewer Logs](images/event-viewer-security-logs.png)

</details>

---

## Tools & Technologies

* Hyper-V
* Windows Server 2019 & Windows 10 Pro
* Active Directory Domain Services & Active Directory Users and Computers
* DNS
* DHCP Server
* Routing and Remote Access Service 
* NAT
* Group Policy Management
* PowerShell
* NTFS Permissions
* Windows File Sharing
* Event Viewer
* Windows Security Logs

---

## Security Concepts Demonstrated

* Active Directory domain deployment
* Centralized identity and access management
* Domain authentication
* Domain Controller administration
* DNS and DHCP integration
* Internal network segmentation
* NAT-based internet access for private clients
* Organizational Unit structure
* Domain user and group administration
* Bulk identity provisioning with PowerShell
* Security groups for access control
* RBAC permission management
* NTFS permissions
* Shared folder authorization
* Least privilege access control
* Group Policy enforcement
* User restriction policies
* Centralized desktop configuration
* Screen lock policy enforcement
* Windows audit policy configuration
* Event log monitoring
* Failed logon monitoring
* Account and group membership change monitoring

---

## Security Monitoring Focus

This lab also included a basic security monitoring component using Windows audit policies and Event Viewer.

The monitoring focused on:

* Failed logon attempts
* Successful logon activity
* Account changes
* User management events
* Group membership changes
* File/share access activity
---

## Insights & Lessons Learned

* Building the environment from scratch helped me understand how Active Directory, DNS, DHCP, NAT, Group Policy, and Windows auditing work together in a domain network.
* Configuring the Domain Controller with both internal and external network adapters helped me understand network segmentation and routing in a virtualized lab.
* Deploying NAT/RRAS demonstrated how internal clients can access the internet through a server acting as a router.
* Configuring DHCP helped me understand how centralized IP address assignment works in a Windows domain.
* Joining a Windows 10 client to the domain demonstrated how authentication works through Active Directory.
* Bulk user creation with PowerShell showed how automation can simplify repetitive identity management tasks.
* Creating security groups and assigning NTFS permissions demonstrated how access can be controlled based on user roles and group membership.
* Testing folder permissions from the client machine helped confirm the practical impact of access control decisions.
* Applying GPOs showed how administrators can centrally enforce user restrictions, desktop settings, and security configurations.
* Configuring audit policies and reviewing Event Viewer logs helped me understand how Windows environments can monitor logons, account changes, and group membership changes.

---

## Copyright Notice

All content and visuals in this repository are original and may not be reused without permission.


## Rakip 

ICT Engineering | Cybersecurity & Network Security

---
