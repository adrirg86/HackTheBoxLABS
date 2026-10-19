# Sequel - DockerLabs 🏴󠁧󠁢󠁥󠁮󠁧󠁿

<img width="601" height="461" alt="image" src="https://github.com/user-attachments/assets/70509306-fd51-41b2-8254-1d83e828183f" />

### 1.  First we prove the conexion and we add to `/etc/hosts`

```bash
ping -c3 10.129.58.171
sudo nano /etc/hosts
```

<img width="523" height="514" alt="image" src="https://github.com/user-attachments/assets/9cafae85-690d-4a81-9db4-da1ce1303b85" />


### 2. Now with nmap we scan all the ports.
```bash
 sudo nmap -sCV 10.129.58.171

```

<img width="941" height="385" alt="image" src="https://github.com/user-attachments/assets/8f790b29-dc88-47a3-ab2b-7aa8c798abc2" />


### 3. We entry to mysql.

```bash
 mysql -h 10.129.58.171 -u root --skip-ssl
```

<img width="728" height="290" alt="image" src="https://github.com/user-attachments/assets/485f517e-7b06-42fa-bd51-96b9ff971177" />


### 4. Now we search for the flag.

<img width="903" height="918" alt="image" src="https://github.com/user-attachments/assets/ea73b99d-d8f8-4606-b7e3-133e2caa09ff" />

