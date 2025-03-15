# Lazy Admin THM CTF


To figure out where the vulnerability is, we must figure out a few key entry points to the system given to us. I ran `nmap -sV <MACHINE-IP>`

![Screenshot 2025-03-15 111106](https://github.com/user-attachments/assets/432a9263-3e70-46ba-a078-25e49b2b8b23)

Nothing out of the usual looks to be happening here. Going to the IP brings me to an Apache default page. The next step in my enumeration process is to use dirbuster
to find any hidden directories I may not know about. Using dirbusters GUI yielded me a countless amount of directories.

![Screenshot 2025-03-15 111835](https://github.com/user-attachments/assets/04597788-6eeb-4433-95e2-f35b042429b6)

I went to the /content/js/SweetRice.js directory and found a comment in the code that said this -> 

`@Package sweetrice `

`@Dashboard core `

`@since 0.5.4`

Looking up "SweetRice 0.5.4 vulnerabilities" leads me to discover CVE-2009-4224, which is a PHP remote file inclusion vulnerability that is found in version 0.5.4
of SweetRice. If you want to read more about this CVE -> https://nvd.nist.gov/vuln/detail/CVE-2009-4224. It looks like we can set up a reverse shell on the system!

Looking back at dirbuster I try different directories until I find /content/as which looks to be a login page and an SQL file. I downloaded the SQL file onto my linux
system for further investigation and it looks like there is credentials in the file.

![Screenshot 2025-03-15 122401](https://github.com/user-attachments/assets/a8dfc199-4da9-469a-bf33-40cf225c7877)

Using ChatGPT, I found that the password was an MD5 hash so I went to a MD5 hash "decryptor" to find the password, which was Password123. Using these credentials I
logged into the SweetRice CMS.

I played around a bit while in the CMS and found that we can enter potentially bad PHP code in the ads section. I copied pentestmonkeys php reverse shell and tried it out -> https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

I was able to upload the malicious code onto the CMS and set up a netcat listener on port 4444. Now the only thing left to do is go to /content/inc/ads and click on the php file I uploaded.

![Screenshot 2025-03-15 164408](https://github.com/user-attachments/assets/9dac5632-867f-4bbc-a82f-ccb3ff67ac0d)


Finding the user flag from here is pretty simple, I used `locate user.txt` in the linux shell and found that the user flag is found in /home/itguy/user.txt

![Screenshot 2025-03-15 164722](https://github.com/user-attachments/assets/5a22eebe-6810-4b62-a428-2dea9316e5d3)


Now to actually escalate to root was a bit of a challenge. in the itguy directory I did sudo -l to figure out how we can run sudo commands, it looks like we can in  /usr/bin/perl /home/itguy/backup.pl. I read the backup.pl file and it pointed to copy.sh, which runs on execution of the .pl file. I couldn't actually edit copy.sh with nano so I used `echo "cp bin/bash /tmp/rootbash; chmod +s /tmp/rootbash" > etc/copy.sh`. What this does right here is puts a command into etc/copy.sh which, when run by the backup.pl file, moves the systems bash shell to /tmp/rootbash, running rootbash with `./rootbash -p` will then allow me to become a root user to eventually run `cat root/root.txt` 

![Screenshot 2025-03-15 171859](https://github.com/user-attachments/assets/c8beef32-3271-4821-8d89-322bb9bce426)

Hope this room was fun for you as much as it was for me! :D

