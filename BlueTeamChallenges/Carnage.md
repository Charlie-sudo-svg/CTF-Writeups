# Carnage Wireshark Challenge

The first thing I did in this challenge is launched the VM from tryhackme and opened the Wireshark application. From there I opened the pcap file the challenge
provides for analysis.

The first question asks, What was the date and time for the first HTTP connection to the malicious IP?

A fairly easy question to start of, nonetheless I typed in a simple query to wireshark to find the answer: `http`. The first thing I immediately notice is a bunch 
of weird looking POST requests but thats besides the point. The first packet is the first connection we see with the malicious IP of 10.9.23.102.

Clicking on the frame in wireshark reveals the time was 2021-09-24 16:44:38

![Screenshot 2025-04-24 175353](https://github.com/user-attachments/assets/15fc72c9-de6b-44fb-bcd1-ff170b370931)
