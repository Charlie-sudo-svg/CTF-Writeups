# Include THM Room

## Step 1: Enumeration

I ran `nmap -sV <MACHINE-IP>` and got a bunch of ports open as seen below: 

![Screenshot 2025-03-21 185827](https://github.com/user-attachments/assets/1d4f6a15-230c-4638-87ed-67e7572cfbc2)

Going to the IP on port 50000 reveals a webpage which I then enumerated directories with gobuster.

Not much of use was found. However, going to port 4000 reveals another login page, which says "Sign In using guest/guest." So I logged in and found this page below!


![Screenshot 2025-03-21 202515](https://github.com/user-attachments/assets/1408eb58-6081-43df-a36b-cef559dafe70)

Clicking on my own profile I can see I have some attributes set to my profile. The first thing I tried was to change isAdmin to true (and it actually worked.) This opened up some new tabs at the top of the screen, those being settings and API. 

The API tab was very interesting, it provided a Get Admins API which responded back with the usernames and passwords of the admins. If only I had a form I could copy and paste the API request into... 

![Screenshot 2025-03-21 203134](https://github.com/user-attachments/assets/a8f35f5c-a114-4904-836e-c62ec21a65e3)

Using a Base64 Decoder I figured out the encoded text said this 

`{"ReviewAppUsername":"admin","ReviewAppPassword":"admin@!!!","SysMonAppUsername":"administrator","SysMonAppPassword":"S$9$qk6d#**LQU"}`

Looks like credentials to me!

Using the SysMon credentials I logged in and found the first flag -> THM{!50_55Rf_1S_d_k3Y??!}

Now the challenge becomes in finding hidden text file in /var/www/html.

I found something interesting in the source code which was /profile.php?img= I think we can do some some of local file inclusion here. I tested a few payloads and changing it to 

`/profile.php?
img=....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2f....%2f%2fetc%2fpasswd`

worked and revealed 2 other users. One of the users that was interesting was Joshua, I ran hyrda with `hydra -l joshua -P rockyou.txt <TARGET_IP> ssh`. Running this yields me the password 123456 for Joshua over SSH. 

Logging in was pretty straight forward, and the last task is to read the hidden file in /var/www/html. I cd'd to there and found the "hidden" file to complete this room.

![Screenshot 2025-03-21 210204](https://github.com/user-attachments/assets/94be348b-215c-4acd-9a63-a3f663626d9e)

Hope you had fun!
