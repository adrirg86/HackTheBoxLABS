# Meow - Hack The Box 🏴󠁧󠁢󠁥󠁮󠁧󠁿

<img width="643" height="446" alt="image" src="https://github.com/user-attachments/assets/ed81a8cd-0687-45f3-86b4-110f74d02188" />


### 1. First we're going to add to `/etc/hosts`

<img width="522" height="218" alt="image" src="https://github.com/user-attachments/assets/714c0e05-3c80-475f-a590-fc7203f1f91b" />

### 2. We will run an nmap scan to see which ports are open.

```bash
nmap -T4 -F 10.129.57.250
```

<img width="638" height="192" alt="image" src="https://github.com/user-attachments/assets/0f728cbb-3724-4c89-ac39-1782b7dca546" />


### 3. We entry with telnet.

```bash
telnet 10.129.57.250
```

<img width="632" height="407" alt="image" src="https://github.com/user-attachments/assets/dac78948-896c-44fb-ad0f-e7222731bd84" />

### 4. We prove the login with `root`.

```bash
user: root
```

<img width="593" height="418" alt="image" src="https://github.com/user-attachments/assets/cf517aa3-1946-49c7-8d35-6a7ce63a6a47" />



