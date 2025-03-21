# Include THM Room

## Step 1: Enumeration

I ran `nmap -sV <MACHINE-IP>` and got a bunch of ports open as seen below: 

![Screenshot 2025-03-21 185827](https://github.com/user-attachments/assets/1d4f6a15-230c-4638-87ed-67e7572cfbc2)

Going to the IP on port 50000 reveals a webpage which I then enumerated directories with gobuster.
