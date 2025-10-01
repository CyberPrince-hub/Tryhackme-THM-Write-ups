TryHackMe - Room:Wgel CTF Writeup Solve Step By Step
Solved By : Prince
Date: 2025-09-30
Difficulty: Easy
The Contents of the Room:
Task 1: User flag
Task 2: Root flag

Tools used:
Nmap
Dirsearch
Gobuster
GTFOBins

TryHackMe | Wgel CTF | Walkthrough
 Hello everyone! In this post, I’ll walk you through the WGELCTF room on TryHackMe. If you’re stuck or need a hint, I hope this step-by-step guide will help you finish the challenge. Let’s dive right in!

Step 1: Initial Recon and Port Scan
As usual, we start by scanning for open ports using nmap. Here's a quick command for that:
nmap -sV -sC -A <IP TARGET> -v
This scan shows two open ports:

22/tcp (SSH)
80/tcp (HTTP - Web Server)
With SSH and a web service running, let’s focus first on port 80 and investigate the web application hosted on the server.

Step 1: Initial Recon and Port Scan
As usual, we start by scanning for open ports using nmap. Here's a quick command for that:

 nmap -sV -sC -vvv -oN nmap-scan ip-add


 
This scan shows two open ports:

22/tcp (SSH)
80/tcp (HTTP - Web Server)
With SSH and a web service running, let’s focus first on port 80 and investigate the web application hosted on the server.

Step 2: Exploring the Web Server
Browsing to http://<target-ip>, you’ll see a website related to career opportunities. This is where we can try some directory brute-forcing to discover hidden files or directories.

I used gobuster to brute force the directories:

 gobuster dir -u http://<target-ip> -w /path/to/wordlist
 Through brute-forcing, I discovered a hidden directory called .ssh. Browsing to http://<target-ip>/.ssh/, I found an id_rsa file, which is the private key used for SSH authentication.

 

Step 3: SSH Access Using id_rsa
Now that we have the id_rsa private key, the next step is to download it and use it to log into the server. Here’s how you can do that:

Right-click on the id_rsa file in your browser and download it to your machine.
Adjust the permissions of the key file:
Step 4: Obtaining the User Flag
Once logged in as jessie, the user flag can typically be found in the home directory. Navigate to it and read the flag:
Step 5: Privilege Escalation
Now, let's move on to privilege escalation to get the root flag.

Running sudo -l to see what commands can be executed with elevated privileges revealed the following:
Step 6: Exploiting Wget to Gain Root Flag
To exploit this, we need to set up a listener on our own machine using netcat:
Replace <your-ip> with the IP address of your machine (you can find it using ifconfig). This command posts the contents of /root/root_flag.txt to our listener. Once the request is sent, you’ll see the contents of the root flag in your terminal where netcat is running.