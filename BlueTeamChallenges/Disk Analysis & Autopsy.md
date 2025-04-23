# Disk Analysis & Autopsy challenge THM

I launched the Windows VM and launched the Autospy application from the desktop.

The first question asks the MD5 has of the .E01 image. To figure this out I went into case files and clicked on HASAN2.e01.txt to reveal the MD5 hash.

![Screenshot 2025-04-22 195535](https://github.com/user-attachments/assets/e84298b8-68d2-4b5a-a7a0-b2a0daa14d7b)

This revealed the hash to be 3f08c518adb3b5c1359849657a9b2079

The next step in my analysis requires me to open up the .aut file using autopsy. From there I located the missing image in my file explorer and opened it up.

![autopsy-fix-missing-image](https://github.com/user-attachments/assets/ebd637a8-0da5-4ef6-84e2-296dc5282535)


After the results open up I navigated to "Operating System Information" and found the computer name was DESKTOP-0R59DJ3 as seen by the image below

![Screenshot 2025-04-22 195839](https://github.com/user-attachments/assets/e2460fd7-9c76-4042-80d2-bef78b04e7c9)

To find all the user accounts I navigated right below OS Information to Operating System User Accounts. There are more user accounts listed than there are answers to the question.
To weed out the accounts that aren't included in the answer I figured that the question only meant user accounts with names and not ones like Administrator and the Guest account.
I then clicked the top of the column to present the options in alphabetical order and figured that the order went as follows -> H4S4N,joshwa,keshav,sandhya,shreya,sivapriya,srini,suba.

![Screenshot 2025-04-22 200306](https://github.com/user-attachments/assets/8e5c1aed-d426-4a43-ab6e-603a3a5700a2)

The next question asked who the last user that logged in was. Using the same tab I did before, I clicked on the column "Date Accessed" to organize users by when they last accessed their computer.
The last person that did was Sivapriya.

I found the IP address of the computer by going to HASAN2.E01/vol3/Program Files (x86)/Look@LAN. From there I scrolled down and found the IP to be 192.168.130.216.

![Screenshot 2025-04-22 201951](https://github.com/user-attachments/assets/fce53652-d309-41e7-96e4-5c1df9c6552a)

Right below the IP address we also see the MAC address as well, 0800272cc4b9. To format this correct we must format it with a dash every 2 characters like this ->
08-00-27-2c-c4-b9.

The name of the network adapter was hard to find. I went to SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkCards and found the name to be Intel(R) PRO/1000 MT Desktop Adapter.

Going through installed programs I found the name of the network monitoring tool was Look@LAN

![Screenshot 2025-04-22 203721](https://github.com/user-attachments/assets/a7e5b0b2-4804-45a9-a7a3-14e740ff3989).

I found what the user bookmarked by going to Web Bookmarks and by finding these coordinates: 12°52'23.0"N 80°13'25.0"E 

![Screenshot 2025-04-22 204008](https://github.com/user-attachments/assets/45372519-09fc-4dc3-bd16-cc6b9bf1d968)


I found the full users name but I was not able to load the picture from autospy due to some sort of glitch not letting me use the applications tab so the screenshot I'm about to provide is from someone elses write up.

Going down the list of users, there were multiple PNG and JPG files, because I couldn't open them I didn't know what the right answer was. After looking up the answer I eventually found
the person I was supposed to focus on was Joshwa. The photo he put on his computer is shared below.

![Screenshot 2025-04-22 204923](https://github.com/user-attachments/assets/fc2ae907-8f3f-445e-b7ea-68ffb65c23ac)

I would just like to add here that this isn't my screenshot. Autospy for some reason isn't working when showing PNGs or JPGs. The full name is Anto Joshwa.

The next question says a user had a file on her desktop. Then she changed the flag using powershell, what was the original flag?

After a little searching around, I found that the user Shreya was the one that had a flag on her desktop. Inspecting the flag file gives us flag{i_changed_it}. This is 
clearly not the right answer but it narrows down our options. 

To make this process quicker I did a keyword search of shreya.txt to see all instances of shreya.txt being used in autopsy. I found one particulary interesting 
artifact which was in ConsoleHost_History.txt. Clicking on it reveals the flag! flag{HarleyQuinnForQueen}

![Screenshot 2025-04-22 210406](https://github.com/user-attachments/assets/b2821183-b02f-476c-bfab-71808343259f)

We still aren't done yet. Apparently the same user found an exploit and used it to escalate priviliges. 

Looking back on Shreyas desktop I found an interesting file called exploit.ps1. Looking inside of the contents of the file I see some sort of exploit code.
It looks like the exploit escalates priviliges and creates a file called hacked.txt. Inside the ps1 file I see this command 

```powershell
Add-Content C:\Users\H4S4N\Desktop\hacked.txt 'Flag{I-hacked-you}'
```

Now I'm no coding expert, but it looks like Shreya adds the flag to the hacked.txt text file.

2 hack tools focused on passwords were found in the system. What are the names of these tools? (alphabetical order)

The first step I took was to go to Web Downloads and I found a download from Github for Mimikatz. 

![Screenshot 2025-04-22 213452](https://github.com/user-attachments/assets/90bfddcd-08b8-4c73-b900-370823fd7eda)

I had a hard time finding the other one, but I eventually landed on looking through alerts Microsoft defender gave me. From there I found an alert, it was lazagne.exe.

![Screenshot 2025-04-22 214727](https://github.com/user-attachments/assets/0cd05abf-1e93-405e-9635-4cb02282ab8b)

The next question asks what the author of a certain YARA file is. I did an exact keyword search of .yar and came across kiwi_passwords.yar.lnk. I didn't find an author
there, but I was able to find where the kiwi_passwords.yar file was located. The file was located in C:\Users\H4S4N\Desktop\mimikatz_trunk\kiwi_passwords.yar. 

I went to that directory and looked at the contents of the file to find the author to the Yara rule was Benjamin DELPY (gentilkiwi).

![Screenshot 2025-04-22 220220](https://github.com/user-attachments/assets/41d5556e-ecfc-4f0d-890b-e62fd805f8a9)

The last question of the task asks, "One of the users wanted to exploit a domain controller with an MS-NRPC based exploit. What is the filename of the archive that you found?"

I did a little googling on what MS-NRPC based exploits on and I found an interesting keyword, zerologon. I did a keyword search and found the last answer ->
2.2.0 20200918 
Zerologon encrypted.zip.

![Screenshot 2025-04-22 221419](https://github.com/user-attachments/assets/528bbb46-9131-458d-9524-9a7c01593556)

Thats it! Thats the entire room, I hope you enjoyed following along. This room better helped me understand how Autopsy works and its impact on computer forensics.
