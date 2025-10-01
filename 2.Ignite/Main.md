TryHackMe - Room:Ignite CTF Writeup Solve Step By Step
Solved By : Prince
Date: 2025-09-30
Difficulty: Easy
I started off this CTF by doing some basic enumeration scans.

Port Scan:
I performed the following port scan:

sudo nmap -vv -sS -sV -sC -oN nmap_out 10.10.62.131
I got only 1 port from the scan:

PORT   STATE SERVICE REASON         VERSION
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
Site Exploration:
I opened the Machine_IP in my browser:
It looks like it is a content-management site hosted by FuelCMS(v1.4).

Scrolled down, found this:
Tried logging in to http://{MACHINE_IP}/fuel with the above credentials:
And success! We are in the admin dashboard!
I was able to access the admin panel but nothing would load. Hopefully nothing here is necessary because I can’t access any of the pages. I looked at some of the Gobuster directories but they also didn’t contain anything interesting. The default page listed the version as 1.4, let’s see if there are any vulnerabilities for this version. We can use Exploit-DB to check for this.

There are three RCEs that we can use on this version of the CMS.
It worked! We have the listed file location from the default page of the website, let’s see if we can use “cat” to display the contents of this file. We’ll send the following command to the machine.

cat fuel/application/config/database.php
From this we get a big piece of information, the root credentials.
We know how to login as root, but we have no way of doing so. We need to try to get a shell in the system so we can change our user to root. Before that though, let’s see if we can submit the user flag using this command interface.

2. Root.txt
We need a way to get a shell in the system, “sudo -l” doesn’t return anything to us. In my last writeup, I used wget to exploit the machine, maybe on this machine we can use it to move a shell from our system onto the target.

We’ll use PentestMonkey’s reverse php shell for our shell. We’ll start by downloading the shell and replacing the IP and port
Next we need to set up an HTTP server on our machine for the target to get the shell from. We can do this by using python, we’ll use the following command.

python3 -m http.server 80
This then will tell us that we’re serving HTTP on port 80.
Now we can start a netcat listener on our machine to catch the shell when we activate it. We can use the following command to start that.

rlwrap nc -lvnp 4444
If you need an explainer on the flags specified in the listener, click here.
We caught the shell! We can’t get to root yet, though, because our shell is not interactive. Let’s upgrade our shell using python. We can run the following command to upgrade.

python -c 'import pty; pty.spawn("/bin/bash")'
Now that we can interact with the shell, let’s use “su root” with the password we found earlier to get root on the machine.
We have root! Let’s grab the flag now and finish up the room!
Lessons Learned:
Python can be used to host a HTTP server
Outdated applications are vulnerable

