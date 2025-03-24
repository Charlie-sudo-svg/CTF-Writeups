# Hammer THM Room

To start the room I enumerated by doing `nmap -sV <MACHINE-IP>`. I noticed only port 22 was open and considering the rooms description mentioned a web application I expanded the range
to include ports 1-5000 just incase by using `nmap -sV -p 1-5000 <MACHINE-IP>`. This yielded an interesting result:

![Screenshot 2025-03-23 220028](https://github.com/user-attachments/assets/3b8622e3-24bc-4518-874f-9fe311bbe1b1)

Port 1337 was open (hahaha....). Knowing this port was open I opened firefox and went to it and I found a GUI to some login page. 

![Screenshot 2025-03-23 220556](https://github.com/user-attachments/assets/84df5b3c-d53a-4967-a068-c6832d88df2e)
