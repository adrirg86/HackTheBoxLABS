# Crocodile - HackTheBox


<img width="602" height="532" alt="image" src="https://github.com/user-attachments/assets/725b7bfc-b3e4-4bde-9f14-e48788184a6d" />


### 1. First we open the machine and we add the host.

```bash
sudo nano /etc/hosts # we add the host
bat /etc/hosts
```

<img width="949" height="436" alt="image" src="https://github.com/user-attachments/assets/b356f8f4-48b3-47de-9f9f-9cded01af5ce" />


### 2. We scan the ports with nmap.

```bash
nmap -T4 -F -sC 10.129.100.147
```

<img width="977" height="516" alt="image" src="https://github.com/user-attachments/assets/1a1a6077-acc7-4e2c-bbf9-5b7d75f21b86" />


### 3. We enter to the ftp port and we get to our computer all the files

```bash
 ftp 10.129.100.147
ls
get allowed.userlist
get allowed.userlist.passwd
exit
```


<img width="956" height="533" alt="image" src="https://github.com/user-attachments/assets/eaeb6b6d-6b75-4894-bc51-896a3fe1f087" />


### 4. We look to our users and password 

```bash
lsd
 bat allowed.userlist
 bat allowed.userlist.passwd
```

<img width="945" height="423" alt="image" src="https://github.com/user-attachments/assets/478fce65-9383-40fe-a47b-e6e38a7122dd" />


### 5. We do a gobuster for search the hidden ports and enter with the user and password.

```bash
gobuster dir -u http://10.129.100.147 -w /usr/share/wordlists/dirb/common.txt -x php,html,txt
```

<img width="955" height="885" alt="image" src="https://github.com/user-attachments/assets/c1c95e39-c160-40a9-9822-473b41e432dd" />



### 6. We get in `http://10.129.100.147/login.php` and we login with 

```
# user: admin
# passwd: rKXM59ESxesUFHAd
```

<img width="960" height="960" alt="image" src="https://github.com/user-attachments/assets/ceefdd18-c5a3-413d-9fcf-5e0087728430" />











