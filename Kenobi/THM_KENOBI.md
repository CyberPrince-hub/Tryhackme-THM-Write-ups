# TryHackMe Writeup – Kenobi

---

## 📌 Executive Summary

This repository contains a detailed writeup for the **Kenobi** room on the TryHackMe platform. The objective of this assessment was to enumerate exposed services, identify security misconfigurations, gain initial access, and escalate privileges to achieve full system compromise.

The lab demonstrates common real-world Linux security issues such as insecure network services, exposed credentials, and vulnerable SUID binaries.

---

## 🧪 Engagement Details

- **Platform:** TryHackMe
- **Room:** Kenobi
- **Difficulty:** Easy
- **Attack Type:** Network Enumeration & Linux Privilege Escalation
- **Attacker Machine:** Kali Linux

---

## 🎯 Scope & Objective

### Scope

- Single Linux target machine hosted on TryHackMe

### Objectives

- Enumerate running services
- Identify misconfigurations
- Gain user-level access
- Escalate privileges to root
- Document impact and mitigations

---

## 🖥️ Target Information

- **Target IP: 10.48.169.234**



## 🛠️ Methodology

The assessment followed a standard penetration testing lifecycle:

1. Service Enumeration
2. Initial Access
3. Privilege Escalation
4. Impact Assessment
5. Mitigation Recommendations

---

## 🔍 1. Service Enumeration

### Network Scanning

```bash
nmap -sC -sV -oN nmap.txt <Target_IP>
```

### Identified Services

| Port | Service |
| ---- | ------- |
| 21   | FTP     |
| 22   | SSH     |
| 80   | HTTP    |
| 111  | RPCBind |
| 139  | SMB     |
| 445  | SMB     |

**Observation:** Multiple file-sharing and remote access services increased the attack surface.

---

## 📂 2. FTP Analysis

```bash
ftp <Target_IP>
```

### Findings

- Anonymous FTP login was enabled
- Internal service-related files were accessible

### Risk

Anonymous FTP access may expose sensitive information useful for further exploitation.

---

## 📡 3. SMB & NFS Enumeration

### SMB Enumeration

```bash
enum4linux -a <Target_IP>
```

### NFS Enumeration

```bash
showmount -e <Target_IP>
```

### Findings

- An NFS export was discovered
- Access controls were improperly configured

### Mounting the NFS Share

```bash
mkdir /mnt/kenobi
sudo mount <Target_IP>:/export /mnt/kenobi
```

Sensitive files were accessible from the mounted directory.

---

## 🔐 4. Initial Access

An exposed **SSH private key** was discovered inside the NFS share.

```bash
chmod 600 id_rsa
ssh -i id_rsa kenobi@<Target_IP>
```

### Access Confirmation

```text
uid=1000(kenobi) gid=1000(kenobi)
```

---

## 🚀 5. Privilege Escalation

### SUID Enumeration

```bash
find / -perm -4000 -type f 2>/dev/null
```

### Vulnerable Binary

```text
/usr/bin/menu
```

### Vulnerability Analysis

- Executes system commands using relative paths
- Does not sanitize the `PATH` environment variable
- Vulnerable to **PATH hijacking**

### Exploitation

```bash
export PATH=/tmp:$PATH
```

Executing the binary resulted in command execution with root privileges.

### Root Access Confirmation

```text
uid=0(root) gid=0(root)
```

---

## 💥 6. Impact Assessment

- Full system compromise
- Unauthorized access to sensitive data
- Ability to modify system configurations
- Potential for persistence and lateral movement

---

## ⚠️ 7. Security Weaknesses Identified

- Anonymous FTP enabled
- Insecure NFS export configuration
- Exposed SSH private key
- Improper SUID binary usage
- Lack of environment variable sanitization

---

## 🛡️ 8. Mitigation Recommendations

- Disable anonymous FTP access
- Restrict NFS exports to trusted hosts
- Secure SSH keys with strict permissions
- Remove unnecessary SUID binaries
- Use absolute paths in privileged programs
- Conduct regular security audits

---

## 🧠 Key Skills Demonstrated

- Network and service enumeration
- SMB and NFS exploitation
- SSH key-based authentication abuse
- Linux privilege escalation (SUID misconfiguration)
- Professional penetration testing documentation

---

## ✍️ Author

**Prince**\
**Focus Area:** Penetration Testing | Linux Security | TryHackMe

Completed Date : 30-01-2026

