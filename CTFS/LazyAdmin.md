# Lazy Admin THM CTF

## Step 1: Enumerate 

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

