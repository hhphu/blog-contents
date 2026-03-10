---
title: "Demystifying Encryption: How It Safeguards Your Messages"
description: "Have you ever wondered how your messages and data stay safe when you send them over the internet? The answer lies in a fascinating world called encryption. In this blog post, we'll explore the basic concepts of encryption in the simplest way possible, leaving the complex math aside."
date: "2023-09-20"
headerImage: "https://miro.medium.com/v2/resize:fit:720/format:webp/1*A2YEGorHtwE1xmQcAiJxiA.jpeg"
tags: []
---

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*A2YEGorHtwE1xmQcAiJxiA.jpeg)

**Understanding the Basics**

Before diving into encryption, let's get familiar with some essential terms:

1. **Cryptographic Algorithm or Cipher:** Think of this as the secret recipe that defines how your message gets transformed into a secret code (ciphertext) and back to its original form (plaintext).

2. **Key:** This is like a secret password that the algorithm needs to work its magic. You use the same key to encrypt and decrypt your messages.

3. **Plaintext:** This is your original message, the one you want to keep secret.

4. **Ciphertext:** This is your message after it's been encrypted, looking like gibberish to anyone who doesn't have the key.

**Symmetric Encryption**

Imagine you and your friend want to send secret messages to each other. With symmetric encryption, you both use the same secret key to encode and decode messages. Here's how it works:

1. **Encryption:** You take your plaintext message and the secret key, and the encryption process magically turns it into ciphertext. This encrypted message is sent over to your friend.

2. **Decryption:** Your friend uses the same secret key you used to decrypt the message and reveal the original plaintext. Without that secret key, no one can make sense of the ciphertext.

**The Evolution of Encryption**

In the past, there was a famous encryption method called DES (Data Encryption Standard). However, as technology advanced, DES became less secure, and it could be broken by attackers. In response, the Advanced Encryption Standard (AES) was introduced in 2001. AES is still widely used today, and it's considered very secure.

AES works its magic by repeating four transformations multiple times:

- **Substitution:** It swaps letters in your message with different ones using a secret table.
- **Shift Rows:** It moves rows of your message around to jumble things up.
- **Mix Columns:** It does some fancy math on each column of your message.
- **Add Round Key:** It adds another layer of secret data to your message.

The total number of these transformations depends on the key size, which can be 128, 192, or 256 bits (don't worry about the bits, it's just a measure of size).

**Keeping Things Simple**

We won't dive into the complex details of AES, but it's essential to know that modern encryption methods like AES are incredibly secure. They ensure:

- **Confidentiality:** Your messages stay private.
- **Integrity:** You can trust that your message hasn't been tampered with.
- **Authenticity:** You know the message came from the person you trust.

**Try It Out**

If you're curious about trying out encryption, you can use tools like GnuPG or OpenSSL. These programs help you encrypt and decrypt your messages easily, keeping your digital conversations secure.

####  GPG
- To encrypt a file
```bash
gpg --symmetric --cipher-algo <CIPHER> <FILE_NAME>
<CIPHER>: the name of the cipher used
<FILE_NAME>: the file to be encrypted
```

The default output is binary gpg format. To create ASCCII output, use --amor option
```bash
gpg --amor --symmetric --cipher-algo <CIPHER> <FILE_NAME>
```

- To decrypt a file
```bash
gpg --output <OUTPUT_FILE> --decrypt <ENCRYPTED_FILE>
```

**Example**
![gpg-demo](https://raw.githubusercontent.com/hhphu/images/main/Demo/Symmetric%20encryption/gpg-demo.png)
From the above screenshot, to encrypt a file named `jokes.txt` using AES-256, we run:
```bash
gpg --symmetric --cipher-algo AES256 jokes.txt
```

Enter a key for encryption. After running the command, we get an output called `jokes.txt.gpg`. To create ASCII output using --armor option, we get `jokes.txt.asc`:
```bash
gpg --armor --symmetric --cipher-algo AES256 jokes.txt
```

#### OpenSSL
- To encrypt a file
```bash
openssl aes-256-cbc -pbkdf2 -iter 10000 -e -in <INPUT_FILE> -out <OIUTPUT_FILE>

aes-256-cbc: the encryption type to be used
-pbkdf2: we use the Password-Based Key Derivation Function 2 to prevent brute force attacks
-iter 10000: Add another layer of protection agains brute-force by deriving the encryption key by specifying the number of iterations
```

- To decrypt a file
```bash
openssl aes-256-cbc -pbkdf2 -iter 10000 -d -in <INPUT_FILE> -out <OUTPUT_FILE>
```

**Example**
![openssl-demo](https://raw.githubusercontent.com/hhphu/images/main/Demo/Symmetric%20encryption/openssl-demo.png)

From the above screenshot, to encrypt a file named `jokes2.txt`, using AES-256, and create output file named `encrypted_jokes2.txt`, we run:
```bash
openssl aes-256-cbc -pbkdf2 -iter 10000 -e -in jokes2.txt -out encrypted_jokes2.txt
```

To decrypted the file and output into `decrypted_jokes2.txt`, we run
```bash
openssl aes-256-cbc -pbkdf2 -iter 10000 -d -in encrypted_jokes2.txt -out decrypted_jokes2.txt  
```

Comparing `jokes2.txt` and `decrypted_jokes2.txt`, we see they have the same content. 

**The Challenge of Symmetric Encryption**

While symmetric encryption is great for a few people sharing secrets, it's not very scalable. Imagine if you had to share unique keys with every friend you wanted to send secret messages to – it quickly becomes unmanageable.

In our next blog post, we'll explore a solution to this scalability challenge with asymmetric encryption, where 100 users can securely communicate with just 100 keys. Stay tuned!
