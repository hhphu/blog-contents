---
title: "A brief look at Man-in-the-Middle Attacks and the Role of Public Key Infrastructure (PKI)"
description: ""
date: "2023-09-22"
headerImage: "https://miro.medium.com/v2/resize:fit:720/format:webp/1*VI_OVPKhWkjFdRjlkXqD3g.png"
tags: []
---

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*VI_OVPKhWkjFdRjlkXqD3g.png)

**Man-in-the-Middle Attack on Diffie-Hellman Key Exchange**

Consider the scenario where Alice and Bob are using the Diffie-Hellman key exchange to establish a secret key. Unfortunately, this exchange is susceptible to MITM attacks, as shown below:

1. Alice and Bob initially agree on two public values, q and g, which can be intercepted by eavesdroppers, including the attacker, Mallory.

2. Alice, as part of the standard protocol, selects a random private variable 'a,' calculates 'A' (A = (g^a) mod q), and sends 'A' to Bob.

3. However, Mallory is ready to intercept Alice's message. Mallory selects her own random private variable 'm' and calculates 'M.' Upon receiving 'A,' Mallory sends 'M' to Bob, masquerading as Alice.

4. Bob, unaware of the malicious interception, continues with the key exchange. He chooses a random private variable 'b,' calculates 'B' (B = (g^b) mod q), and sends 'B' to Alice.

5. Yet again, Mallory intercepts Bob's message, retrieves 'B,' and sends 'M' to Alice, still pretending to be Bob.

6. Both Alice and Bob calculate the shared secret key using 'M' (key = M^a mod q for Alice, and key = M^b mod q for Bob).

This elaborate MITM attack leaves Alice and Bob oblivious to the fact that they are communicating with Mallory instead of each other. Mallory can read and manipulate their messages at will.

**The Need for Identity Confirmation: Public Key Infrastructure (PKI)**

To counteract MITM attacks, a mechanism for confirming the identities of communication parties is essential. This is where Public Key Infrastructure (PKI) comes into play.

Consider a scenario where you are browsing a website, such as example.org, over HTTPS. How can you be sure that you are genuinely communicating with the legitimate example.org server and not falling victim to a MITM attack? The answer lies in the website's SSL/TLS certificate.

**The Role of SSL/TLS Certificates**

When browsing a secure website, your browser often displays a lock icon, indicating that the connection is secured over HTTPS. At the time of writing, example.org is using a certificate signed by DigiCert Inc., signifying the certificate's validity until a specific date.

For a certificate to be signed by a Certificate Authority (CA) like DigiCert Inc., the following steps are typically involved:

1. **Generate a Certificate Signing Request (CSR):** You create a certificate and send your public key to a CA to be signed. The alternative, self-signing, is generally insecure.

2. **Send CSR to a Certificate Authority:** The CA signs your certificate, providing validation. Trust in the CA is crucial for this system to work.

This trust is reflected in your browser, which recognizes and trusts specific CAs, as shown by the list of trusted certificate authorities.

**Generating a Certificate Signing Request with OpenSSL**

To generate a Certificate Signing Request, you can use OpenSSL with the following command:

```openssl req -new -nodes -newkey rsa:4096 -keyout key.pem -out cert.csr```

This command generates a CSR and prompts you to answer a series of questions to identify your certificate.

**Creating a Self-Signed Certificate with OpenSSL**

For testing purposes, you can create a self-signed certificate using the following command:

```openssl req -x509 -newkey -nodes rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365```

This command generates a self-signed certificate valid for one year.

**Conclusion**

In summary, while Diffie-Hellman key exchange is a robust method for secure key generation, it remains susceptible to MITM attacks. To confirm the identities of communicating parties and mitigate this threat, Public Key Infrastructure (PKI) is essential. SSL/TLS certificates signed by trusted Certificate Authorities (CAs) play a pivotal role in establishing trust in online communication, ensuring secure and authenticated data exchanges between users and servers. Understanding these principles is crucial for anyone navigating the complexities of secure communication in the digital age.
