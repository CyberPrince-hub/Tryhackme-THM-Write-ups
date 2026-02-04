# 🚀 TryHackMe Writeup – VulnNet:Node

> A Easy and Best penetration testing writeup for the **VulnNet:Node** room on TryHackMe.
![VulnNet](https://github.com/user-attachments/assets/78406f49-c6d5-4c62-bb6a-1bd166adf9bf)

---

## 📌 Executive Summary

This repository documents the exploitation of the **VulnNet:Node** machine on TryHackMe. The objective was to enumerate exposed services, identify vulnerabilities in a Node.js web application running on **port 8080**, gain initial access, and escalate privileges to obtain **root access**.

This lab demonstrates real-world security issues such as:

* 🌐 Vulnerable Node.js web applications
* 🧩 Insecure deserialization
* 🔑 Weak credential handling
* 🐧 Linux privilege escalation misconfigurations

---

## 🧪 Engagement Details

| Item        | Details                                       |
| ----------- | --------------------------------------------- |
| Platform    | TryHackMe                                     |
| Room        | VulnNet:Node                                  |
| Difficulty  | Medium                                        |
| Attack Type | Web Exploitation & Linux Privilege Escalation |
| Attacker OS | Kali Linux                                    |

---

## 🎯 Scope & Objective

### 📍 Scope

* Single Linux target hosted on TryHackMe

### ✅ Objectives

* Enumerate open ports and services
* Exploit a vulnerable Node.js application
* Gain user-level access
* Escalate privileges to root
* Document impact and mitigations

---

## 🖥️ Target Information

* **Target IP:** `10.48.184.186`

---

## 🛠️ Methodology

The assessment followed a standard penetration testing lifecycle:

1. 🔍 Service Enumeration
2. 🌐 Web Application Analysis
3. 🔐 Initial Access
4. 🚀 Privilege Escalation
5. 💥 Impact Assessment
6. 🛡️ Mitigation Recommendations

---

## 🔍 1. Service Enumeration

### 🔎 Network Scanning

```bash
nmap -sC -sV -p- -oN nmap.txt 10.48.184.186
```

### 📊 Discovered Services

| Port | Service        |
| ---- | -------------- |
| 22   | SSH            |
| 8080 | HTTP (Node.js) |

**Observation:** A Node.js web application was exposed on **port 8080**, which became the primary attack surface.

---

## 🌐 2. Web Application Enumeration

The web application was accessed via:

```
http://<Target_IP>:8080
```

### Findings

* The application accepted user-controlled input
* Serialized data was processed by the backend
* Input validation and security controls were missing

This behavior suggested a possible **insecure deserialization vulnerability**.

---

## 💉 3. Vulnerability Analysis

### Insecure Deserialization (Node.js)

The Node.js application insecurely handled serialized objects supplied by the user. By modifying serialized data, it was possible to alter backend execution flow.

This vulnerability allowed **remote command execution** on the target system.

---

## 🔐 4. Initial Access

Using the insecure deserialization flaw, arbitrary system commands were executed, resulting in a reverse shell on the target machine.

### 🟢 Access Confirmation

```text
uid=1000(node) gid=1000(node)
```

User-level access was successfully obtained.

---

## 🚀 5. Privilege Escalation

### 🔎 Local Enumeration

```bash
sudo -l
```

### Finding

The compromised user was allowed to execute a script or binary with elevated privileges.

### Exploitation

By abusing the misconfigured sudo permission, commands were executed as **root**, resulting in full privilege escalation.

### 🔴 Root Access Confirmation

```text
uid=0(root) gid=0(root)
```

---

## 💥 6. Impact Assessment

* Full system compromise
* Remote code execution via web application
* Unauthorized privilege escalation
* High risk of persistence and lateral movement

---

## ⚠️ 7. Security Weaknesses Identified

* Vulnerable Node.js application on port 8080
* Insecure deserialization of user input
* Lack of proper input validation
* Misconfigured sudo permissions

---

## 🛡️ 8. Mitigation Recommendations

* Avoid unsafe deserialization in Node.js applications
* Implement strict input validation and sanitization
* Apply the principle of least privilege
* Review and restrict sudo permissions
* Conduct regular application security testing

---

## 🧠 Key Skills Demonstrated

* Network and service enumeration
* Node.js web application exploitation
* Insecure deserialization attacks
* Linux privilege escalation
* Professional penetration testing reporting

---

## ⚠️ Disclaimer

This writeup is for **educational purposes only** and was performed in a **legally authorized lab environment**.

---

## ✍️ Author

**Prince**
Completed Date: 4-02-2026
*Penetration Testing | Web Security | Linux Privilege Escalation | TryHackMe*

