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

I found the administrators email by using the websites naming conventions for emails. When looking at the "We are Hiring" post on the website we see a JD@anthem.com. I used this naming convention to find the email SG@anthem.com

Now we are on to finding the flags, I went to /umbraco and entered in the SG@anthem.com aswell as the UmbracoIsTheBest! that I found in robots.txt, thinking it could be a potentially leaked password. I was right!

![Screenshot 2025-04-07 165606](https://github.com/user-attachments/assets/1d26ec19-338e-4e06-8a7f-d724acfd7ee7)

Using the website, I was able to look through the content and meta tags to find the first flag.

![Screenshot 2025-04-07 165804](https://github.com/user-attachments/assets/518f779b-0d6e-4fdd-9821-4905486f81f0)

I actually found the 3rd flag before I found the second one by looking at the authors category in the content page under author URL.

![Screenshot 2025-04-07 165922](https://github.com/user-attachments/assets/c4007387-de7e-4dc6-88d6-655814202be4)

I found the 4th flag in the content page aswell in the archives.

![Screenshot 2025-04-07 170100](https://github.com/user-attachments/assets/227bb11c-a06e-4855-8741-da1eae8dd60e)

And last but not least, the 2nd flag was found when I hit inspect, it was on the search bar the entire time.


![Screenshot 2025-04-07 170408](https://github.com/user-attachments/assets/a47fa947-2899-4c26-b2de-ba51178af98c)

## Final Stage

The final stage requires us to RDP into the desktop. I remotely accessed the desktop using rdesktop with the credentials SG and UmbracoIsTheBest!. I knew SG was the right username because it tells us the computer is not connected to a domain.

From here I was able to find the flag in User.txt

![Screenshot 2025-04-07 171103](https://github.com/user-attachments/assets/7e3b3115-8cda-4782-8e7f-52ef6785113c)

From here we know there is a hidden file somewhere within the system. To uncover it, I went to file explorer -> View -> Options -> View -> Show Hidden Files.

After doing this a file called backup appears in the C: drive. We dont have the permissions to open the file. To change that, I went to the file properties, then security to added everyone to the file.

![Screenshot 2025-04-07 171932](https://github.com/user-attachments/assets/c14ae579-f479-422f-a981-c61d6efce843)

After this, I was able to successfully retrive the flag from the file.

To find the last flag, I had to do a bit of searching, after figuring out the password for the administrator account was right in front of me, I exited out and started a new RDP session with rdesktop and logged in as Administrator using flag 3 as the password. 

![Screenshot 2025-04-07 172341](https://github.com/user-attachments/assets/6511f35f-efb2-4647-b4c5-ec4cc637ae30)
