# Tusk Info Stealer Lab Cyber Defenders

Downloading the file provided by Cyber Defenders provides me with a SHA256 hash of E5B8B2CF5B244500B22B665C87C11767. This hash is supposed to be used to answer all related questions
questions below.

The first question asks how big is the file in KB. To answer this, I uploaded the hash into virus total and got 921.36KB.

Doing a quick google search reveals the second answer is mammoth, which is what the hacker uses to refer to victims.

On the same website to answer the second question we find the answer to the third question

![Screenshot 2025-04-19 003032](https://github.com/user-attachments/assets/0801d04a-4664-410c-8b57-aa03cb30c816)

tidyme.io

Back on this website -> https://securelist.com/tusk-infostealers-campaign/113367/

I found that they used dropbox to host the samples for MacOS and Windows.

![Screenshot 2025-04-19 003159](https://github.com/user-attachments/assets/7cf530bc-16ed-4219-9bc2-95a0b45257f7)

The website then explains that the password extracted from the configuration file is newfile2024

![Screenshot 2025-04-19 003312](https://github.com/user-attachments/assets/b4e7f170-aa31-49bd-b7ce-135f6e2a33fb)

Right below that paragraph we see the function downloadAndExtractArchive is used to extract the field archive from the config file.

I also figured out from the article that the name of the translator they copied was yous.ai and they copied the site and named it voico.io

Going down to the network IOCs in the article we find that the ip addresses for the C2 server are as follows -> 46.8.238.240 and 23.94.225.177.

![image](https://github.com/user-attachments/assets/c503717f-c735-4bbf-b430-42e0a6f42c73)

Scrolling down once more I found the address of the ETH wallet which was 0xaf0362e215Ff4e004F30e785e822F7E20b99723A.

![image](https://github.com/user-attachments/assets/21a3e90e-03ee-4b12-9a9c-eb9c89b1bb7e)

Although thats the official end of the lab in Cyber Defenders, I decided to look up the ETH address on etherscan.io and found a large 8 ETH ($13,000.00 USD) was transfered from the account.

This was an interesting lab that mainly tested your research skills. Not too technical, but it was interesting learning about how this malware was able to ravage many people.
