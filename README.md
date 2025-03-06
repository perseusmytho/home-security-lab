# 🏠 Home Networking & Cybersecurity Lab

## 📌 Overview
This project showcases a **home networking and cybersecurity lab** designed to master **network configuration, user authentication, access control, and intrusion detection/prevention**. Built in a virtualized environment, it leverages **pfSense** as the router/firewall, **Active Directory (AD)** for domain management, and a mix of client and pentesting VMs to simulate real-world attack scenarios. Key features include **firewall rules, AD group policies, file sharing, and advanced IDS/IPS** with Suricata, culminating in successful **pentesting exploits** against a vulnerable target.

## 🛠 Tools & Technologies Used
- **pfSense** – Router, firewall, and IDS/IPS (Suricata)  
- **Active Directory (ADUC)** – User and group management  
- **Windows Server 2022** – Domain Controller for `lab.local`  
- **Windows 10** – Domain-joined client  
- **Kali Linux** – Penetration testing platform  
- **Metasploitable** – Vulnerable target VM  
- **VirtualBox** – Virtualization platform  

## 🔍 Key Findings & Implementation

| Feature             | Description                                  | Implementation                                                                                     |
|---------------------|----------------------------------------------|---------------------------------------------------------------------------------------------------|
| **Network Configuration** | Virtual LAN with routing and DHCP.           | Configured pfSense with WAN (NAT) and LAN (“intnet”, `192.168.1.1`), assigned IPs (`192.168.1.x`) via DHCP. |
| **Active Directory Domain** | Centralized authentication and management. | Promoted Server 2022 to DC for `lab.local`, joined Win10, created users (`LAB\jdoe`, `LAB\jsmith`) in "Staff" OU. |
| **Group Policy (GPO)** | Enforced domain-wide security policies.     | Applied `StaffPolicy` to "Staff" OU, set minimum password length to 8, verified on Win10.         |
| **File Sharing**    | AD-managed shared resources.                | Set up `\\WIN-1CJM1RVQU0H\StaffDocs` on Server 2022, granted `LAB\jdoe` and `LAB\jsmith` Read/Write access. |
| **Firewall & IDS/IPS** | Network security and threat detection.    | Configured pfSense firewall to block Kali (`192.168.1.103`) SMB to Server (`192.168.1.102`), deployed Suricata (replaced Snort) for IDS/IPS with Emerging Threats rules. |
| **Pentesting**      | Simulated attacks on AD and vulnerable targets. | Used Kali to scan Server (`nmap -A 192.168.1.102`) and exploit Metasploitable (`vsftpd_234_backdoor`, `usermap_script`), generating alerts like “ET EXPLOIT” and “INDICATOR-COMPROMISE”. |

## 🔒 Lab Security Measures
This lab implements robust security through:
- **Firewall Rules**: Blocked Kali’s SMB access (`192.168.1.103:445` to `192.168.1.102`) while allowing Win10 file sharing—tested with `crackmapexec` failures and `\\WIN-1CJM1RVQU0H\StaffDocs` success.
- **Intrusion Detection/Prevention**: Switched from Snort to Suricata after dependency issues—configured with `emerging-scan.rules` and `emerging-exploit.rules`, suppressed noise (e.g., QUIC alerts), and detected Nmap scans and Metasploit exploits.
- **Access Control**: Enforced GPOs (e.g., password policy) and file share permissions—restricted `LAB\jdoe` and `LAB\jsmith` to "Staff" resources.
- **Pentesting Validation**: Exploited Metasploitable with Kali (e.g., VSFTPd backdoor, Samba `usermap_script`)—Suricata flagged root access (“INDICATOR-COMPROMISE id check returned root”) and attack responses.

## ✅ Key Takeaways & Security Best Practices
🔹 **Centralized Authentication** via AD streamlines management and bolsters security—tested with domain logins and GPOs.  
🔹 **Least Privilege** via GPOs and permissions limits exposure—verified with restricted file access.  
🔹 **Advanced IDS/IPS** with Suricata outperforms initial Snort setup—caught scans (e.g., “ET SCAN Behavioral unusual port 445 traffic”) and exploits (e.g., “ET EXPLOIT VSFTPd Backdoor Attempt”).  
🔹 **Pentesting** with Kali against Metasploitable validated defenses—successful exploits (Samba shell on `192.168.1.104:5555`) proved detection reliability.  
🔹 **Traffic Analysis** and suppression (e.g., QUIC noise) sharpened focus on critical threats—enhanced alert clarity.  
