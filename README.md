# 🏠 Home Networking & Cybersecurity Lab

## 📌 Overview
This project demonstrates a **home networking lab** built to explore **network configuration, user authentication, access control, and cybersecurity**. It features a virtualized environment with **pfSense** as the router/firewall, **Active Directory (AD)** for domain management, and a mix of client and target VMs to simulate real-world scenarios. The lab covers **firewall rules, AD group policies, file sharing, and intrusion detection/prevention**.

## 🛠 Tools & Technologies Used
- **pfSense** – Router, firewall, and IDS/IPS (Snort)  
- **Active Directory (ADUC)** – User and group management  
- **Windows Server 2022** – Domain Controller for `lab.local`  
- **Windows 10** – Domain-joined client  
- **Kali Linux** – Penetration testing  
- **Metasploitable** – Vulnerable target VM  
- **VirtualBox** – Virtualization platform  

## 🔍 Key Findings & Implementation

| Feature | Description | Implementation |
|---------|-------------|----------------|
| **Network Configuration** | Virtual LAN with routing and DHCP. | Configured pfSense with WAN (NAT) and LAN (“intnet”, `192.168.1.1`), assigned IPs (`192.168.1.x`) to VMs via DHCP. |
| **Active Directory Domain** | Centralized authentication and management. | Promoted Server 2022 to DC for `lab.local`, joined Win10, created users (`LAB\jdoe`, `LAB\jsmith`) in "Staff" OU. |
| **Group Policy (GPO)** | Enforced domain-wide security policies. | Applied `StaffPolicy` to "Staff" OU, set minimum password length to 8, verified on Win10. |
| **File Sharing** | AD-managed shared resources. | Set up `\\WIN-1CJM1RVQU0H\StaffDocs` on Server 2022, granted `LAB\jdoe` and `LAB\jsmith` Read/Write access. |
| **Firewall & IDS** | Network security and threat detection. | Configured pfSense firewall to block Kali (`192.168.1.103`) SMB to Server (`192.168.1.102`), deployed Snort for IDS. |
| **Pentesting** | Simulated attacks on AD and vulnerable targets. | Used Kali to scan Server (`nmap -A 192.168.1.102`) and exploit Metasploitable (`vsftpd_234_backdoor`). |

## 🔒 Lab Security Measures
This lab emphasizes proactive security through:
- **Firewall Rules**: Blocked specific threats like Kali’s SMB access to AD while preserving legitimate traffic.
- **Intrusion Detection**: Deployed Snort on pfSense to monitor and log attack attempts from Kali.
- **Access Control**: Enforced GPOs and file share permissions to restrict user privileges.

## ✅ Key Takeaways & Security Best Practices
🔹 **Centralized Authentication** via AD simplifies user management and enhances security.  
🔹 **Least Privilege** enforced through GPOs and file share permissions prevents over-access.  
🔹 **Network Segmentation** and IDS (Snort) improve threat detection and mitigation.  
🔹 **Regular Testing** with tools like Kali ensures configurations hold against attacks.  
