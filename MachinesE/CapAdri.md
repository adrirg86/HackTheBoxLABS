### Cap-HackTheBox🏴󠁧󠁢󠁥󠁮󠁧󠁿

**First we need to prove to send ping**
```bash
ping -c3 10.129.3.209
```

<img width="960" height="298" alt="image" src="https://github.com/user-attachments/assets/5ad8e65a-8cd7-4cda-b9a5-969e4f684647" />


**1. How many TCP ports are open?**

*We are going to use nmap to find the open ports*.
```bash
nmap -p- 10.129.3.209
```

<img width="963" height="275" alt="image" src="https://github.com/user-attachments/assets/a8aef030-4372-4f40-919e-0da8d2814ecd" />

*Results: 3*


**2. After running a "Security Snapshot", the browser is redirected to a path of the format /[something]/[id], where [id] represents the id number of the scan. What is the [something]?**

*To get the result, we look at the URL*

<img width="970" height="304" alt="image" src="https://github.com/user-attachments/assets/6a6fe427-2249-43d8-96bc-1733ffbb6512" />

*Results: data*


**3. Are you able to get to other users' scans?**

<img width="956" height="738" alt="image" src="https://github.com/user-attachments/assets/b3ee4efc-4221-4994-a22a-f0990030633e" />


*Results: yes*


**4. What is the ID of the PCAP file that contains sensative data?**

*There is a button on the website to download the Wireshark capture.*

<img width="947" height="784" alt="image" src="https://github.com/user-attachments/assets/d9c269b1-f8b6-4208-b507-09d0b8a97f62" />


*We look the ID = 0*

<img width="1786" height="998" alt="image" src="https://github.com/user-attachments/assets/b293fd53-ec1d-4ebf-b2c2-11a0db571e88" />


*Results: 0*


**5. Which application layer protocol in the pcap file can the sensetive data be found in?**

*Results: ftp*

*In Wireshark, we need to find the first HTTP signal and follow it in TCP protocol mode, changing the sequence from 0 to 1 to 2 to 3. The password and username will then be displayed.*

<img width="954" height="1022" alt="image" src="https://github.com/user-attachments/assets/bda2c473-8e33-414f-b3ac-e0142704bec0" />

**6. We've managed to collect nathan's FTP password. On what other service does this password work?**

*Results: ssh*
```bash
ssh nathan@10.129.3.209
passwd: Buck3tH4TF0RM3!
```

<img width="961" height="839" alt="image" src="https://github.com/user-attachments/assets/21d93ec3-f30e-4e49-b647-fac108edea78" />


**7. Submit the flag located in the nathan user's home directory.**

*The user's flag is in the user's directory and we read it with cat*
```bash
 cat user.txt
```

<img width="994" height="235" alt="image" src="https://github.com/user-attachments/assets/df6cec1a-d94c-40c8-bf4d-41ebf902cf9b" />


**8. What is the full path to the binary on this machine has special capabilities that can be abused to obtain root privileges?**
*Let's look at the binaries we can run*
```bash
getcap -r / 2>/dev/null
```
<img width="964" height="184" alt="image" src="https://github.com/user-attachments/assets/feb853b2-8067-4027-b29c-4d99e30bb239" />


**9. Submit the flag located in root's home directory.**
*We're going to escalate privileges to root by running the binary file we saw earlier, assuming we have permission.*

<img width="980" height="154" alt="image" src="https://github.com/user-attachments/assets/165731ba-f592-498a-ab26-d9b3c741e2fd" />

*Finally, we're going to look for the root flag.*


<img width="965" height="329" alt="image" src="https://github.com/user-attachments/assets/03e248c2-501b-4ac0-8c91-076d03bc46f3" />



<img width="634" height="543" alt="image" src="https://github.com/user-attachments/assets/e4cfead0-8060-4908-b367-645a671dd70f" />

