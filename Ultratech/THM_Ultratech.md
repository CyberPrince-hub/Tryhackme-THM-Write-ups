# 🚀 TryHackMe Writeup – UltraTech

> A clean and beginner‑friendly penetration testing walkthrough of the **UltraTech** room on TryHackMe.![Ultratech](https://github.com/user-attachments/assets/c8fb64e6-7bc6-4d33-a5cd-9a6f760514ad)


---

## 📌 Executive Summary

This repository documents the step‑by‑step exploitation of the **UltraTech** machine on TryHackMe.
The objective was to identify exposed services, exploit a vulnerable web application and database, gain initial user access, and finally escalate privileges to obtain **root access**.

This lab demonstrates real‑world security issues such as:

* 🌐 Vulnerable web applications
* 🗄️ Exposed databases
* 🔑 Credential reuse
* 🐳 Docker‑based privilege escalation

---

## 🧪 Engagement Details

| Item        | Details                                       |
| ----------- | --------------------------------------------- |
| Platform    | TryHackMe                                     |
| Room        | UltraTech                                     |
| Difficulty  | Medium                                        |
| Attack Type | Web Exploitation & Linux Privilege Escalation |
| Attacker OS | Kali Linux                                    |

---

## 🎯 Scope & Objective

### 📍 Scope

* Single Linux target hosted on TryHackMe

### ✅ Objectives

* Enumerate open ports and services
* Exploit web application vulnerabilities
* Extract and crack credentials
* Gain SSH access
* Escalate privileges to root

---

## 🖥️ Target Information

* **Target IP:** `10.49.163.33`

---

## 🛠️ Methodology

The following penetration testing lifecycle was followed:

1. 🔍 Service Enumeration
2. 🌐 Web Application Analysis
3. 🔐 Credential Extraction
4. 🚪 Initial Access
5. 🚀 Privilege Escalation
6. 💥 Impact & Mitigation

---

## 🔍 1. Service Enumeration

### 🔎 Network Scanning

```bash
nmap -sC -sV -oN nmap.txt <10.49.163.33>
```

### 📊 Discovered Services

| Port  | Service |
| ----- | ------- |
| 21    | FTP     |
| 22    | SSH     |
| 80    | HTTP    |
| 8081  | HTTP    |
| 27017 | MongoDB |

⚠️ **Observation:** MongoDB was exposed without authentication, which is a critical security issue.

---

## 🌐 2. Web Application Enumeration

The web application was accessible on **ports 80 and 8081**.

Checking the `robots.txt` file revealed a hidden directory:

```text
/utechadmin
```

Opening this path displayed an **admin login panel**.

---

## 💉 3. Web Vulnerability Analysis

### SQL Injection

The admin login page was vulnerable to **SQL Injection**, allowing authentication bypass and database access.

Using crafted payloads, valid usernames and password hashes were extracted from the backend database.

---

## 🗄️ 4. Database Enumeration (MongoDB)

MongoDB was running without authentication.

```bash
mongo <Target_IP>
```

### 📂 Commands Used

```javascript
show dbs
use utech
show collections
db.users.find()
```

### ✅ Result

* Usernames and password hashes were obtained
* Hashes were cracked offline using **John the Ripper**

---

## 🔐 5. Initial Access

The recovered credentials were reused to gain SSH access:

```bash
ssh r00t@<Target_IP>
```

### 🟢 Access Confirmation

```text
uid=1001(r00t) gid=1001(r00t)
```

---

## 🚀 6. Privilege Escalation

### 🔎 Checking Sudo Permissions

```bash
sudo -l
```

### ❗ Finding

The user was allowed to execute **Docker commands** with sudo privileges.

### 🐳 Docker Exploitation

```bash
sudo docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

This command mounted the host filesystem inside a Docker container and provided a **root shell**.

### 🔴 Root Access Confirmation

```text
uid=0(root) gid=0(root)
```

---

## 💥 7. Impact Assessment

* Complete system compromise
* Unauthorized access to database data
* Credential exposure and reuse
* Root‑level command execution
* High risk of persistence and lateral movement

---

## ⚠️ 8. Security Weaknesses Identified

* Exposed MongoDB service without authentication
* SQL Injection vulnerability in the web login
* Credential reuse across services
* Misconfigured sudo permissions for Docker

---

## 🛡️ 9. Mitigation Recommendations

* Restrict database services to localhost
* Enable authentication on MongoDB
* Sanitize and validate all user input
* Enforce strong and unique passwords
* Limit Docker and sudo permissions
* Perform regular security audits

---

## 🧠 Key Skills Demonstrated

* Network and service enumeration
* Web application exploitation
* MongoDB enumeration
* Password hash cracking
* Docker‑based privilege escalation
* Professional penetration testing documentation

---

---

## ✍️ Author

**Prince**
Completed date: 4-02-2026
*Penetration Testing | Web Security | Linux Privilege Escalation | TryHackMe*

