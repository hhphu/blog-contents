---
title: "TryHackMe - Daily Bugle"
description: "Compromise a Joomla CMS account, cracking hashes and escalating privileges by taking advantage of yum."
date: "2023-04-09"
headerImage: "https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20DailyBugle/Header.png"
tags: TryHackMe
---

---

- ***Title:*** Daily Bugle
- ***Description:*** Compromise a Joomla CMS account via SQLi, practice cracking hashes and escalate your privileges by taking advantage of yum.

---

### Reconnaissance
- Run `nmap` to gather the target's information
```bash
	nmap -A -Pn $IP >> $IP-nmap-scan
```

![nmap-scan](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20DailyBugle/nmap-scan.png)

- From the nmap scan, I see port 80 is open:

![web-server](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20DailyBugle/web-server.png)

- What's more, I found some potential directories to exploit and figured that the web server is using Joomla.

- Doing some Googling, I found a way to find Joomla version by accessing the following url:
_https://www.itoctopus.com/how-to-quickly-know-the-version-of-any-joomla-website_

- Also from this link, I was able to find a lot of directories for possible exploits. Joomla version can also be found in README.md

### Exploit
- I found a vulnerability related to Joomla 3.7.0 [CVE-2017-8917]("https://www.exploit-db.com/exploits/42033")
Using a python script from [HarryR's github]("https://github.com/XiphosResearch/exploits/tree/master/Joomblah"), I was able to retrieved the login credentials for the CMS.

```bash
	python3 joomlah.py http://$IP
```

![joomlah-script](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20DailyBugle/joomlah-script.png)

- Found a user "jonah" with a hash password, which uses bcrypt algorithm. Use John to crack this hash:

```bash
	john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=bcrypt
```

![crack-hash-password](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20DailyBugle/crack-hash-password.png)

- We're now able to log into Joomla admin as `jonah`

- Following instruction on this [article]("https://www.hqphu.com/posts/6H6RATKcCXxBmtS027AfKO"), I was able to gain a reverse shell on the machine. Running linpeas.sh on the target machine, I found a password in config file:

![user-password](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20DailyBugle/user-password.png)

- Using this password, we're able to SSH to the target machine under jjameson.
- A simple command ```sudo -l```, we found jjameson can run yum. Refering to [GTFOBins]("https://gtfobins.github.io/gtfobins/yum/"), i was able to escalate to root.

![priv-esc](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20DailyBugle/priv-esc.png)

