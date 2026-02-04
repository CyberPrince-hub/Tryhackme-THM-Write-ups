# 🏴‍☠️ TryHackMe Writeup – Lian_Yu

---

## 📌 Executive Summary

This repository contains a penetration testing writeup for the **Lian_Yu** room on **TryHackMe**. The objective was to enumerate exposed services, uncover hidden web content, extract credentials, gain initial access to the system, and escalate privileges to obtain root access.

This lab focuses on **web enumeration, steganography, credential discovery, and Linux privilege escalation**, making it a great beginner-to-intermediate level challenge.

---

## 🧪 Engagement Details

* **Platform:** TryHackMe
* **Room:** Lian_Yu
* **Difficulty:** Easy
* **Attack Type:** Web Enumeration & Linux Privilege Escalation
* **Attacker Machine:** Kali Linux

---

## 🎯 Scope & Objective

### Scope

* Single Linux target machine hosted on TryHackMe

### Objectives

* Enumerate running services
* Discover hidden web directories
* Extract credentials from files
* Gain SSH access
* Escalate privileges to root

---

## 🖥️ Target Information

* **Target IP:** `10.49.163.216`

---

## 🛠️ Methodology

The assessment followed a standard penetration testing lifecycle:

1. Service Enumeration
2. Web Application Enumeration
3. Credential Discovery
4. Initial Access
5. Privilege Escalation
6. Impact & Mitigation

---

## 🔍 1. Service Enumeration

### Network Scanning

```bash
nmap -sC -sV -oN nmap.txt 10.49.163.216
```

### Identified Services

| Port | Service |
| ---- | ------- |
| 21   | FTP     |
| 22   | SSH     |
| 80   | HTTP    |

**Observation:** A web server and FTP service were exposed, indicating possible file leaks and hidden content.

---

## 🌐 2. Web Enumeration

### Website Analysis

The HTTP service on port 80 displayed a basic webpage with minimal information.

### Directory Brute Forcing

```bash
gobuster dir -u http://<10.49.163.216> -w /usr/share/wordlists/dirb/common.txt
```

### Findings

* Hidden directories discovered
* One directory contained image and text files referencing **Lian Yu** characters

---

## 🖼️ 3. Steganography & File Analysis

An image file was downloaded and analyzed for hidden data.

### Steganography Analysis

```bash
steghide info image.png
steghide extract -sf image.png
```

### Result

* Hidden text file extracted
* File contained encoded credentials

The credentials were decoded and revealed a valid username and password.

---

## 🔐 4. Initial Access

Using the discovered credentials, SSH access was obtained.

```bash
ssh <username>@<Target_IP>
```

### Access Confirmation

```text
uid=1000(user) gid=1000(user)
```

---

## 🚀 5. Privilege Escalation

### Sudo Permission Check

```bash
sudo -l
```

### Finding

The user had permission to run certain commands with sudo privileges.

### Exploitation

By abusing the allowed sudo command, a root shell was obtained.

### Root Access Confirmation

```text
uid=0(root) gid=0(root)
```

---

## 💥 6. Impact Assessment

* Complete system compromise
* Unauthorized access to sensitive files
* Privilege escalation to root
* Potential for persistence

---

## ⚠️ 7. Security Weaknesses Identified

* Hidden sensitive files exposed via web server
* Weak credential storage
* Lack of file access restrictions
* Improper sudo configuration

---

## 🛡️ 8. Mitigation Recommendations

* Remove sensitive files from web directories
* Use strong credential management
* Restrict sudo permissions
* Apply proper file access controls
* Conduct routine security audits

---

## 🧠 Key Skills Demonstrated

* Nmap service enumeration
* Web directory brute forcing
* Steganography analysis
* SSH-based initial access
* Linux privilege escalation
* Professional security documentation

---

---

## ✍️ Author

**Prince Roy**
Completed Date: 4-02-2026
**Focus Area:** Penetration Testing | Web Security | Linux Privilege Escalation | TryHackMe
