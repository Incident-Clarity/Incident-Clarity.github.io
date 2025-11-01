---
author:
- aayan
- test
categories:
- Linux
- PGP
- Security
- Privacy
date: "2023-06-14T01:21:31+05:30"
draft: false
showauthor: true
tags:
- blog
title: My PGP key
---

> Date: 14th June, 2023

## My public PGP key:

[PGP_KEY]https://incidentclarity.qzz.io/PGP_KEY.txt


## Or get it from here: 

https://keys.openpgp.org/vks/v1/by-fingerprint/361B51C910B491E96A668AE5DC06AA3111421839

---

## Steps to send a message using PGP on Linux:

- Copy the public key into a file, example: `recipientkey.asc`

- Import the public key using this command: `gpg --import recipientkey.asc`

- When you import the key, there is an *email* associated with it, **note it**.

- Now encrypt your message with my PGP key: `gpg --encrypt --sign --armor -r email_you_noted@example.com message.txt`

## How to Decrypt an encrypted message?

- This can only be performed by the person who has the **Private key**: `gpg --decrypt message.txt.asc > decrypted_message.txt`

