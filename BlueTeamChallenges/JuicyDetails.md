# Juicy Details Blue Team Challenge

This is a great challenge that tested my analyst skills.

To start off, I had to download the task files which were logs. Unfortunately, it wasn't a pcap file, it was a TXT file, meaning I had to sift through all the files
manually.

The first question asks for the tools the attacker used. The first step I did was open the access.txt file from the task files and already immediately saw nmap was being
used. The rest of the tools will be posted below as screenshots.

![Screenshot 2025-04-17 220249](https://github.com/user-attachments/assets/7fb5fe84-88d1-46fb-9c47-dc748ff091ab)

![Screenshot 2025-04-17 220523](https://github.com/user-attachments/assets/7a732190-b3ca-4fcc-8931-8caca04cfa31)

![Screenshot 2025-04-17 220720](https://github.com/user-attachments/assets/94846778-83c6-4adf-99c3-dbb0e0fd7201)

![Screenshot 2025-04-17 220853](https://github.com/user-attachments/assets/fe03875e-107a-452d-b0e8-ba9d1d71763f)

![Screenshot 2025-04-17 220857](https://github.com/user-attachments/assets/0fbb8212-9618-4f73-b087-564ca177363d)

The answer I came up with was nmap, hydra, sqlmap, curl, feroxbuster in that order. 

Going back through the logs to when hydra was used, I was able to find the directory that was vulnerable to the brute force attack. In this case, that directory was /rest/user/login.

![Screenshot 2025-04-17 221104](https://github.com/user-attachments/assets/0aa12b6c-0ab4-487d-b9be-e198feec4e12)

To find the endpoint that was vulnerable to the sql attack, I simply went back to where sqlmap was used and found it was used in /rest/products/search

![image](https://github.com/user-attachments/assets/09a42a42-cd92-40f4-89e0-ab248fc07d6f)

## Task 3 Stolen Data
I found the parameter used by looking at the same log and saw the parameter used was Q.

The last question in task 2 asks what endpoint the attacker tries to use to extract the files. Looking at the last few logs tells me that the attacker used /ftp as shown below.

![Screenshot 2025-04-17 222040](https://github.com/user-attachments/assets/a2fff88b-fc2c-49b5-afb5-4acbc92dcba0)
