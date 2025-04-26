# Carnage Wireshark Challenge

The first thing I did in this challenge is launched the VM from tryhackme and opened the Wireshark application. From there I opened the pcap file the challenge
provides for analysis.

The first question asks, What was the date and time for the first HTTP connection to the malicious IP?

A fairly easy question to start of, nonetheless I typed in a simple query to wireshark to find the answer: `http`. The first thing I immediately notice is a bunch 
of weird looking POST requests but thats besides the point. The first packet is the first connection we see with the malicious IP of 10.9.23.102.

Clicking on the frame in wireshark reveals the time was 2021-09-24 16:44:38

![Screenshot 2025-04-24 175353](https://github.com/user-attachments/assets/15fc72c9-de6b-44fb-bcd1-ff170b370931)

We can also find the next answer to the next question. The next question asks what was the name of the zip file downloaded. Looking at the same query, I saw a get request to /incidunt-consequatur/documents.zip.
So the answer to the question is documents.zip

![Screenshot 2025-04-25 175608](https://github.com/user-attachments/assets/d42ec4ff-fc73-447e-bc0c-b1af0954537e)

Further looking at the packet details also reveals the domain hosting the file which was attirenepal.com.

To find the name of the file without downloading the zip file I looked in the second packet and found this 

![Screenshot 2025-04-25 181815](https://github.com/user-attachments/assets/39589f64-0e67-48d0-9bcf-e7a063ae837c)

chart-1530076591.xls is the answer I was looking for. 

The same packet also reveals what the webserver is under Server. The server is litespeed and to find the version I right clicked the packet and clicked "Follow TCP Stream" From there I found it was running with PHP/7.2.34

![Screenshot 2025-04-25 182104](https://github.com/user-attachments/assets/bdc8bd3d-7637-45a7-a712-297e2fe2199f)

I had to use the hint to find the malicious domains, I found them between the times of 16:45:11 and 16:45:30. The domains required me to follow quite a few TCP streams and doing alot of looking around. The domains I found were finejewels.com.au, thietbiagt.com, new.americold.com.

I found the CA that issued the SSL certificate for finejewels.com.au by going back to that packet and following the TCP stream.

![Screenshot 2025-04-25 182423](https://github.com/user-attachments/assets/1e1b180c-b430-4305-981d-2a64892be700)

The next step was to find the 2 IP addresses that were associated with C2 servers. I put in a filter to filter only for HTTP GET requests and looked at conversations. From there I brute forced my way to the answer, checking virus total for each IP until I found evidence of C2.

The 2 IPs I found were 185.106.96.158, 185.125.204.174 as evident by the screenshots below.


![Screenshot 2025-04-25 185316](https://github.com/user-attachments/assets/c5ba35b7-62c0-4f02-a50c-2bfcba440da6)

![Screenshot 2025-04-25 185332](https://github.com/user-attachments/assets/d7b126f0-ecf3-4b38-ad74-38dcd4a71333)

Next, I found the host header by looking at the packet details where it said "Host" on packets with the IP of 185.106.96.158 and found oscp.verisign.com.

The next question asks what is the domain name name for the first IP address of the C2 server? This is fairly easy. I know the first C2 server is 185.106.96.158.
I filtered for DNS but decided it would be easier to filter by clicking on statistics -> resolved addresses. From here I filtered by hosts and scrolled down until I found the IP.

![Screenshot 2025-04-26 130532](https://github.com/user-attachments/assets/561c00cb-5b8c-445d-a390-7d4b71f29942)

After that, the next question asked for the second domain name so I used the same process I just mentioned to find the other domain name.

![image](https://github.com/user-attachments/assets/ea213a4e-0b6d-406e-bc06-40da0261b0b7)

What is the domain name of the post infection traffic?

For this question, I used the query http.request.method == POST because thats where the post infection traffic begins. From there I saw that maldivehost.net was the domain.

![Screenshot 2025-04-26 131420](https://github.com/user-attachments/assets/eea709e8-3e9a-4b87-8a02-9788699ad493)

The next 3 questions are pretty easy, it asks the first 11 characters the host sends to the domain and looking at the first post request we see the first 11 characters are zLIisQRWZI9. The lenght of the packet is 281 and can also be found in the screenshot above.

To find the server header, I right clicked on the packet and clicked "Follow TCP Stream" and found the server header: Apache/2.4.49 (cPanel) OpenSSL/1.1.1l mod_bwlimited/1.4.

![Screenshot 2025-04-26 132314](https://github.com/user-attachments/assets/0b858553-f711-4653-a663-607d136dc08f)

The next questions asks about an API that was invoked to find the IP address of the victims machine. I tried to filter for DNS but there was too many, I actually sifted through and found the APIs name which is api.ipify.org. To filter down the search, I used "dns and frame contains "api""

![image](https://github.com/user-attachments/assets/96546b09-e2d0-45c9-b983-7bec56145b89)

Looks like there was some malicious spam (malspam) activity going on. What was the first MAIL FROM address observed in the traffic?

This was also a fairly easy question. I filtered this by typing `Frame contains "MAIL FROM"` in wireshark and found the address farshin@mailfa.com.

![image](https://github.com/user-attachments/assets/78ec2eaa-a97b-449f-b1e8-287c8e040198)

Last but not least, the last question asks how many packets were observed for SMTP traffic. I filtered for SMTP traffic by typing in `SMTP` in wireshark and observed 1439 by clicking on statistics -> capture file properties, then I viewed how many packets were displayed.

![image](https://github.com/user-attachments/assets/ab6d0d0d-d32a-43de-b174-7119549af9e2)

This was a great wireshark exercise to refresh my memory on wireshark and I highly recommend it to everyone trying to improve on their analysis skills!


