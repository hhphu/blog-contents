---
title: "TryHackMe - Overpass 2"
description: "Analyze PCAP file to investigate attackers' actions and hack back to the system."
date: "2023-07-04"
headerImage: "https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20OverPass%202/OverPass-Header.png"
tags: TryHackMe
---

---

- ***Title:*** Overpass 2
- ***Description:*** Overpass has been hacked! Analyze PCAP file to investigate attackers' actions. From there try to hack back in the system.

---

### Task 1 - Forensic - Analysis the PCAP

- By filtering the packets by ```http```, I can easily see the URL of the page attackers used to upload the reverse shell. The packet I was looking for was the one with POST request.

![POST Request](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20OverPass%202/POST-Request.png)

- Following HTTP Stream, I can figure all information that tracks down attackers' actions.

![POST HTTP Stream](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20OverPass%202/POST-HTTP-Stream.png)

- Filtering the PCAP file using ```tcp contains password```, I was able to retrieve 5 users with 5 hashes.

![users-hashes](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20OverPass%202/users-hashes.png)

- Using John with fasttrack.txt, I was able to crack 4 passwords for 4 users: 

![cracked-passwords](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20OverPass%202/cracked-passwords.png)

### Task 2 - Research & Analyze the code

- In the HTTP Stream, I found that attackers downloaded code from this [page](https://github.com/NinjaJc01/ssh-backdoor). Analyzing the code on the git hub page, we'll find all the answers. After analyzing the codes, I was able to retrieve the default hash for the backdoor, the hardcoded  salt for the backdoor and the has that the attacker used (from the PCAP file). From there, I was able to crack the password using the list rockyou.txt. Combining the found hash and salt, I was able to crack the password for the backdoor (SHA512($p.$s))

### Task 3 - Attack & Get Back in!
- Visiting the website on port 80, we can easily see the message by the attacker.

- Using the credentials found in Task 2, to ssh to the machine through the backdoor. 

```bash
    ssh james@$IP -p 2222
``` 

![ssh-backdoor](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20OverPass%202/ssh-backdoor.png)

- Running some directory exploration, I found user.txt under james' home folder. I also found .suid_bash file, which has suid bit set. This can be used to escalate privilege to root.

```bash
    ls -la /home/james
```
![james-home.png](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20OverPass%202/james-home.png)

- Referencing [GTFOBin]("https://gtfobins.github.io/"), I was able to get root by running the following command:

```bash
    ./.suid_bash -p
```

![get-root.png](https://raw.githubusercontent.com/hhphu/images/main/THM%20-%20OverPass%202/get-root.png)

And we're done for the room. With root access, I could get both user's flag and root flag.

