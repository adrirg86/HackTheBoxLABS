# Redeemer - Hack The Box🏴󠁧󠁢󠁥󠁮󠁧󠁿

<img width="621" height="444" alt="image" src="https://github.com/user-attachments/assets/b904fc52-15b1-41c3-9ead-4b52cb04ee0a" />


### 1. First we add to `/etc/hosts`.

<img width="587" height="437" alt="image" src="https://github.com/user-attachments/assets/c185a11f-f730-4f17-b590-aefc10e9c1a0" />

### 2. Now we are going to complete perform a port scan using Nmap.

sudo: Essential for using -sS (SYN scan) or achieving maximum performance, as it requires root privileges to craft raw packets efficiently.

-T4: Increases overall timing and speed (you can try -T5, but it can be overly aggressive and drop ports if there is packet loss).

--min-rate 1000: Forces Nmap to send at least 1,000 packets per second, which drastically speeds up scans on stable networks (like Hack The Box).

-n: Disables reverse DNS resolution, saving time by not attempting to resolve hostnames.

-Pn: Skips the host discovery phase (assumes the host is online), preventing stalls if the target ignores ICMP pings.

```bash
sudo nmap -p- -T4 --min-rate 1000 -n -Pn 10.129.58.145
```

<img width="755" height="253" alt="image" src="https://github.com/user-attachments/assets/e4c21879-b171-426b-8ffa-fe0c25801ebd" />


### 2. We entry to `redis`

```bash
redis-cli -h  10.129.58.145 -p 6379
```

<img width="684" height="307" alt="image" src="https://github.com/user-attachments/assets/851daa16-9fbd-4ec3-a684-9693cb7bcfd8" />


### 3. We can look the information with `info`

<img width="641" height="452" alt="image" src="https://github.com/user-attachments/assets/d7a6799f-a996-4cdc-beab-5d1bc0c81470" />


### 4. From the SELECT command onwards, we listed all keys using KEYS * and retrieved the flag value using the GET flag command.

<img width="737" height="605" alt="image" src="https://github.com/user-attachments/assets/cad20dbb-6c54-489d-a020-19c313bf833d" />

