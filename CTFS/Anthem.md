# Anthem THM Room

This room wants me to exploit a Windows machine, lets get started!

I first used nmap to try to find the port of the webserver, I found that the answer was port 80. The only 2 ports on this machine that are open are port 80 and port 3389.

![Screenshot 2025-04-07 154804](https://github.com/user-attachments/assets/25b2e918-7bd5-4e98-a5a9-b7c4b0460e92)

The port used for RDP is 3389.

To find the answer to the next question I had to check <MACHINE-IP>/robots.txt. In the directory I found UmbracoIsTheBest! so I used that as the answer for what the web crawlers
try to look for.

To find the CMS, I can also look at robots.txt and found umbraco, Googling it I found that umbraco is a CMS. I also found the domain by going to the bottom of the page of `/`
and found anthem.com.

Finding the administrators name was a little bit harder, looking at the poem posted on the main page of the website I googled and found the name Solomon Grundy.

![Screenshot 2025-04-07 160637](https://github.com/user-attachments/assets/45263ef2-13e9-490b-b2c6-168ec8cfcd51)
