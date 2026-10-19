# Fawn - DockerLabs🏴󠁧󠁢󠁥󠁮󠁧󠁿

<img width="642" height="404" alt="image" src="https://github.com/user-attachments/assets/b21b81d9-da8b-4921-af4a-d46a072f954b" />


### 1. First we put it inside of `/etc/hosts`

<img width="863" height="459" alt="image" src="https://github.com/user-attachments/assets/bec37dd0-9f0f-4af4-804b-ea62b5376086" />

### 2. Now we are going to fast perform a port scan using Nmap.

```bash
nmap -T4 -F 10.129.58.140
```

<img width="533" height="217" alt="image" src="https://github.com/user-attachments/assets/d163666f-e529-4b8c-9869-52f9d4eaff4b" />


### 3. We access via FTP.

```bash
ftp 10.129.58.140
# user: anonymous
# passwd: admin
```

<img width="655" height="223" alt="image" src="https://github.com/user-attachments/assets/070f087e-eb62-4895-a5f3-9d213dc884a8" />


### 4. We get the FLAG.

<img width="934" height="277" alt="image" src="https://github.com/user-attachments/assets/2299f92e-f15e-41f9-8cfa-4dd3cdd7e535" />

