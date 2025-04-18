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

I found the parameter used by looking at the same log and saw the parameter used was Q.

The last question in task 2 asks what endpoint the attacker tries to use to extract the files. Looking at the last few logs tells me that the attacker used /ftp as shown below.

![Screenshot 2025-04-17 222040](https://github.com/user-attachments/assets/a2fff88b-fc2c-49b5-afb5-4acbc92dcba0)

## Task 3 Stolen Data

The first question asks what section of the website the attacker tried to scrape email addresses from. Looking at the top of the access log tells us the whole story.

![Screenshot 2025-04-18 144053](https://github.com/user-attachments/assets/4033149e-c777-4827-8087-67a996ae4036)

We can see they went to product reviews to try to find email addresses.

The next question asks if the brute force was successful. To find this, I looked in the section of the log where hydra was used and in fact they did get a 200 response meaning it was successful.

![Screenshot 2025-04-18 144238](https://github.com/user-attachments/assets/a71312d1-6324-4cbd-b2e5-c8e2ce0cde3d)

The user information the hacker was able to collect from using sqlmap was emails and passwords as shown by the log below.

![Screenshot 2025-04-18 144503](https://github.com/user-attachments/assets/4b0285b4-bb06-4711-90fa-c5ff45c2629b)

Whoever the hacker was, tried to download multiple files using FTP including the ones shown below

![Screenshot 2025-04-18 144728](https://github.com/user-attachments/assets/e6ace323-fef7-4979-9410-fe6861cbc993)

Now, finding the service and account name used to retrieve those files, we have to go to a different log file. vsftpd log file is the important one to look at here.
Looking at the logs below, I figured that ftp allowed anonymous logins and therefore my answer included anonymous.

![Screenshot 2025-04-18 145010](https://github.com/user-attachments/assets/a1f4719c-bf39-4650-8767-3254695450fb)

Last but not least, the service and the username that gained shell access is also pretty easy to find. I navigated to the auth logs and found that the service used
was ssh and the username was www-data.

![image](https://github.com/user-attachments/assets/7adafccf-0ddf-4368-a2b1-0ffb1572534e)


## Conclusion

This challenge showed the importance of careful log analysis, knowing what indicators to look for, and connecting the dots to see the full picture. I definitely learned a lot and feel more confident in my ability to handle similar incidents in a real-world environment. Looking forward to tackling more challenges like this!
