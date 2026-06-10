![Status](https://img.shields.io/badge/status-complete-brightgreen)

## Active Directory Enterprise Lab: AD DS, NAT, DHCP, GPOs, RBAC File Shares, Security Auditing and Backup

A Windows Server lab that demonstrates the deployment and administration of an Active Directory environment using Hyper-V, Widnows Server 2019, AD DS, DNS, DHCP, NAT/RRAS, Group Policies, RBAC file sharing, Windows security auditing (Event Viewer) and Active Directory System State backup and recovery.

---

## Project Summary

This project simulates a enterprise Windows domain environment built from scratch in Hyper-V. I configured a Windows Server 2019 machine as a Domain Controller, created an Active Directory domain, deployed DHCP and NAT/RRAS, joined a Windows 10 client to the domain, created 1000+ users with a Powershell script and security groups, configured shared folder permissions, applied Group Policy Objects, and enabled audit and event monitoring so I can view this specific logs in Event Viewer. I also created an Active Directory System State backup and tested the restore process to confirm that the Domain Controller could be recovered successfully. The goal of this lab was to understand how core Microsoft infrastructure services work together in a domain environment, including deployment, administration, access control, monitoring and recovery.

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

* Created the lab environment using Hyper-V.
* Downloaded Windows Server 2019 and Windows 10 ISOs.
* Created a Windows Server 2019 virtual machine.
* Created a Windows 10 Pro client virtual machine.
* Connected the Windows 10 client to the internal Hyper-V switch.

### 2. Domain Controller Deployment

* Renamed the Windows Server machine to `RAKIPDC`.
* Configured a static IP address on the server.
* Installed the Active Directory Domain Services role.
* Promoted the server to a Domain Controller.

### 3. NAT/RRAS and Internal Networking

* Added a second internal Hyper-V virtual switch network adapter to the Domain Controller.
* Configured the internal adapter with a separate private subnet.
* Installed and configured Routing and Remote Access Service.
* Enabled NAT so the internal Windows 10 client could access the internet through the Domain Controller.
* Verified connectivity from the client machine using cmd network tests.

### 4. DHCP Configuration

* Installed the DHCP Server role
* Created a DHCP scope for the internal network based on the Internal LAN Iadapter subnet.
* Configured the Domain Controller’s internal IP as the default gateway for clients.
* Verified that `CLIENT1` received an IP address automatically from the DHCP server scope.
* Checked DHCP leases from the server to see that everything works fine.

### 5. Client Domain Join

* Installed Windows 10 Pro on the client VM
* Joined the Windows 10 client to the `rakip.com` domain and verified connectivity to it by confirming that the client computer object appeared in Active Directory.
* Verified that the client received DHCP configuration.

### 6. Active Directory User and Group Management

* Added an administrative user to the Domain Admins group
* Used a PowerShell script to bulk-create more than 1,000 users
* Created Organizational Units (OUs) inside Active Directory

### 7. RBAC-Style File Sharing with Security Groups

* Created security groups for different access levels and departments.
* Configured Windows file sharing.
* Applied NTFS permissions to folders based on Active Directory security groups.
* Tested access from the Windows 10 domain joined client.
* Verified that users could only access the folders they were authorized to access by the departments they were part of.
* Confirmed that unauthorized access attempts were blocked.

### 8. Group Policy Configuration

* Created Group Policy Objects using Group Policy Management.
* Configured a GPO to block access to Control Panel and Settings for selected users.
* Configured a centralized wallpaper policy using a shared UNC network path.
* Configured screen saver settings Enabled password protection.
* Configured automatic screen lock after 5 minutes of inactivity.
* Configured a logon banner/message GPO to display a warning message before the user sign in.
* Linked GPOs to OUs and groups.
* Verified that the policies applied successfully on the client machine.

### 9. Security Auditing and Event Monitoring

* Configured audit policies for security monitoring.
* Monitored authentication and account related events in Event Viewer.
* Used the Event Viewer filtering to inspect security events.
* Reviewed failed logon activity.
* Reviewed user and account changes.
* Reviewed group membership changes.

The monitoring is focused on:

* Failed logon attempts.
* Successful logon activity.
* Account changes.
* User management events.
* Group membership changes.
* File/share access activity.
---

## Screenshots

![Active Directory Lab Architecture](images/AD-LAB-ARCHITECTURE.png)

<details>
<summary>🔎 View Full Lab Walkthrough (Screenshots)</summary>

### 1. Installing Windows Server 2019 for the Domain Controller and Windows 10  for the client VM.

![Windows Server Installation](images/WindowsServer2019.png)
![Windows 10 Installation](images/Windows10.png)

### 2. Hyper-V VM Domain Controller Creation.

![VM Creation](images/DC.png)
![VM Creation](images/DC2.png)
![VM Creation](images/DC3.png)

### 3. Renaming the Server before promoting it to a Domain Controller.

![Rename Server](images/Rename1.png)

### 4. Configuring a static IP address on the Ethernet WAN adapter of `RAKIPDC` to support AD DS, DNS, DHCP, and NAT/RRAS services.

![Static IP Configuration](images/StaticIP-EthernetWAN.png)
![Static IP Configuration](images/STATIC-EXT.png)

### 5. Installing Active Directory Domain Services.

![Install AD DS](images/AD1.png)
![Install AD DS](images/AD2.png)

### 6. Promoting the Server to Domain Controller.

![Promote Domain Controller](images/PROMOTEDC.png)
![Promote Domain Controller](images/PROMOTEDC2.png)
![Promote Domain Controller](images/PROMOTEDC3.png)

### 7.The Domain rakip.com is ready to use.

![Create Domain](images/DC-READY.png)

### 8. Active Directory Users and Computers.

![ADUC](images/ADUC.png)

### 9. Creating an Organizational Unit, adding a domain user with administrative privileges, and logging in with that user to verify the account.

![OU Users](images/OU.png)
![OU Users](images/LOGIN.png)

### 10. Installing the Routing and Remote Access role on `RAKIPDC` to configure NAT/RRAS for internet access from the internal client network later on.

![Install RRAS](images/RAS1.png)

### 11. Creating an internal Hyper-V virtual switch to isolate the client network and connect `CLIENT1` VM to the private domain network.

![Internal Switch](images/INTERNAL-SW.png)
![Internal Switch](images/INTERNAL-SW2.png)

### 12. Adding an internal adapter to `RAKIPDC` and giving it a static IP for the private network.

![Internal Adapter](images/INTERNAL-SW4.png)
![Internal Adapter](images/INTERNAL-SW3.png)
![Internal Adapter](images/STATIC-INT.png)

### 13. Configuring NAT/RRAS and selecting the Ethernet WAN adapter as the public interface for the `RAKIPDC` so it can route internal client traffic to the internet.

![NAT RRAS](images/NAT1.png)
![NAT RRAS](images/NAT2.png)

### 14. DHCP Server Installation

![Install DHCP](images/DHCP1.png)

### 15. Configuring the DHCP scope `10.0.0.100–10.0.0.200` based on the internal adapter subnet so `CLIENT1` can automatically receive a valid IP address inside the private domain network.

![DHCP Scope](images/DHCP2.png)
![DHCP Scope](images/DHCP3.png)
![DHCP Scope](images/DHCP4.png)
![DHCP Scope](images/DHCP5.png)

### 16. Windows 10 Client VM Setup. I connected this VM with the internal adpater we created for the private network.

![Windows 10 Client](images/CLIENT1.png)
![Windows 10 Client](images/CLIENT1(2).png)

### 17. Verifying that `CLIENT1` receives an IP address from the internal LAN DHCP scope and confirming that the `rakip.com` domain is reachable from the client machine.

![Client DHCP](images/CMD1.png)
![Client DHCP](images/CMD2.png)
![Client DHCP](images/CMD3.png)

### 18. Joining CLIENT1 to the Domain so it can connect with Active Directory.

![Join Domain](images/CLIENT-DC.png)
![Join Domain](images/CLIENT-DC2.png)
![Join Domain](images/CLIENT-DC3.png)

### 19. Verifying the DHCP lease on `RAKIPDC` and checking Active Directory Users and Computers to confirm that `CLIENT1` appears in the Computers container.

![DHCP Lease](images/DCHP-VERIFICATION.png)
![DHCP Lease](images/DC-VERIFICATION.png)

### 20 . Automated user creation with PowerShell. Opening PShell as an admin and running the script.

![Bulk User Creation](images/BULK1.png)
![Bulk User Creation](images/BULK2.png)
![Bulk User Creation](images/BULK3.png)
![Bulk User Creation](images/BULK4.png)

### 21. Created Users in Active Directory

![Created Users](images/BULK5.png)
![Created Users](images/BULK6.png)
![Created Users](images/BULK7.png)

### 22. Connecting to `CLIENT1` as one of the users I created with the script above. Notepad name `Rakip me` gets automatically created as user `rme`.

![PowerShell User](images/RME1.png)
![PowerShell User](images/RME2.png)

### 23. Creating Security Groups for file access control and adding at least one user to each group for permission testing.

![Security Groups](images/SECGROUPS1.png)
![Security Groups](images/SECGROUPS2.png)

### 24. Creating department folders inside `CompanyFiles` and sharing the main folder so domain users can access the permitted folders over the network.

![Department Folders](images/SHARED1.png)
![Shared File](images/SHARED2.png)

### 25. New Technology File System (NTFS) Permission Configuration.

![NTFS Permissions](images/NTFS1.png)
![NTFS Permissions](images/NTFS2.png)
![NTFS Permissions](images/NTFS3.png)
![NTFS Permissions](images/NTFS4.png)

### 26. Testing folder permissions from `CLIENT1` by logging as a domain user and confirming that only the authorized shared folder is accessible.

![Authorized Folder Access](images/TEST1.png)
![Authorized Folder Access](images/TEST2.png)
![Authorized Folder Access](images/TEST3.png)

### 27. Navigating to Group Policy Management to create my GPOs.

![Group Policy Management](images/GPOs.png)

### 28. GPO: Screen Lock Policy. Password & Account restrictions.

![GPO](images/PASS1.png)
![GPO](images/PASS2.png)

### 29. Testing the password GPO and finding out it works because after 3 failed log in attempts it locks the account. Also there are policy restrictions when you want to change the user password (length etc.).

![GPO](images/PASS3.png)
![GPO](images/PASS5.png)
![GPO](images/PASS4.png)

### 30. GPO: Control Panel and Settings Restriction. Also testing GPO.

![Control Panel and Settigns GPO](images/SET1.png)
![Control Panel and Settigns GPO](images/SET2.png)
![Control Panel and Settings GPO](images/SET3.png)
![Control Panel and Settings GPO](images/SET4.png)

### 31. GPO: Centralized Wallpaper. Configuring a centralized wallpaper GPO using the shared network path `\\RAKIPDC\Shares\Wallpaper` so selected domain users receive the same desktop background.

![Wallpaper GPO](images/WALL1.png)
![Wallpaper GPO](images/WALL2.png)
![Wallpaper GPO](images/WALL3.png)
![Wallpaper GPO](images/WALL4.png)

### 32. GPO: Banner when the user tries to log in.

![Banner](images/BAN1.png)
![Banner](images/BAN2.png)
![Banner](images/BAN3.png)

### 33. GPO: Security Logs. Helps us reviewing Windows Security logs in Event Viewer to monitor authentication activity, account changes, and other important security events in the domain environment.

![Audit GPO](images/SEC1.png)
![Audit GPO](images/SEC2.png)
![Audit GPO](images/SEC3.png)
![Audit GPO](images/SEC4.png)

### 34. Linking this GPO to my domain and forcing an group policy update from the cmd in both DC and my `CLIENT1`.
![CMD GPO UPDATE](images/CMD-EVENTVIEWER.png)

### 35. Also enabling auditing on the CompanyFiles shared folder so Windows could log successful and failed access attempts, such as read, write and execute activity by domain users.

![SHARDE FOLDER AUDITING](images/FOLDER-AUDITING.png)

### 36. Reviewing Windows Security logs in Event Viewer to verify that audit events are being recorded for logons, account changes, group membership changes, and shared folder access activity.

![EVENT VIEWER AUDITING](images/EVENT-VIEWER-RESULTS.png)


</details>

---

## Tools & Technologies

* Hyper-V
* Windows Server 2019 & Windows 10 Pro
* Active Directory Domain Services & Active Directory Users and Computers
* DNS
* DHCP 
* Routing and Remote Access Service (RRAS)
* Network Address Translation (NAT)
* Group Policy Management
* PowerShell
* NTFS Permissions
* Windows File Sharing
* Event Viewer
* Windows Security Logs
* Windows Server Backup

---

## Security Concepts Demonstrated

* Active Directory domain deployment.
* Centralized identity and access management.
* Domain authentication.
* Domain Controller administration.
* DNS and DHCP integration.
* Internal network segmentation.
* NAT based internet access for private clients.
* Organizational Units structure.
* Domain user and group administration.
* Bulk identity provisioning with PowerShell.
* Security groups for access control.
* RBAC permission management.
* NTFS permissions.
* Shared folder authorization utilizing least privilege access control.
* Group Policy enforcement.
* User restriction policies.
* Centralized desktop configuration.
* Screen lock policy enforcement.
* Windows audit policy configuration.
* Event log monitoring.
* Failed logon monitoring.
* Account and group membership change monitoring.
* Active Directory System State backup
* Recovery testing

---

## Insights & Lessons Learned

* Building the environment from scratch helped me understand how Active Directory, DNS, DHCP, NAT, Group Policies, and Windows auditing work together in a network.
* Configuring the Domain Controller with both internal and external network adapters helped me understand network segmentation and routing.
* Deploying NAT/RRAS demonstrated how internal clients can access the internet through a server acting as a router.
* Configuring DHCP helped me understand how centralized IP address assignment works in a Windows domain.
* Joining a Windows 10 client to the domain demonstrated how authentication works through Active Directory.
* The Bulk user creation with PowerShell showed how automation can simplify repetitive identity management tasks.
* Creating security groups and assigning NTFS permissions demonstrated how access can be controlled based on user roles and group membership.
* Testing folder permissions from the client machine helped confirm the practical impact of access control decisions.
* Applying GPOs showed how administrators can centrally enforce user restrictions, desktop settings, and security configurations.
* Configuring audit policies and reviewing Event Viewer logs helped me understand how Windows environments can monitor important system activities.
* Creating and restoring an Active Directory System State backup helped me understand the importance of backup and recovery for Domain Controllers.

---

## Copyright Notice

All content and visuals in this repository are original and may not be reused without permission.


## Rakip 

ICT Engineering | Cybersecurity & Network Security

---
