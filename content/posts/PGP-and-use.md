---
author: Aayan
categories:
- Linux
- PGP
- gnupg
date: "2023-10-29T13:36:30+05:30"
draft: false
showTableOfContents: true
showauthor: true
tags:
- blog
- PGP
title: GnuPG and it's use
url: /PGP-and-use/
---

> Date: 29th October, 2023

## About and instalation of GPG (GNU Privacy Guard)

**GPG (GNU Privacy Guard) is a complete and free implementation of the OpenPGP standard.**

URL: https://www.gnupg.org/

On Archlinux using yay: `yay -S core/gnupg`


---

## Encryption / Decryprion 

Encrypt file or folder (compress the folder) (**Self Encrypt**): 
```sh
gpg --encrypt --recipient self@self.com folfer-or-file.tar.gz
```

Encrypt for multiple people to decrypt: 
```sh
gpg --encrypt --recipient notself@notself.com --recipient someone@example.com xyz.txt
```

Decrypt to terminal: 
```sh
gpg  --decrypt snap.xyz.gpg
```

Decrypt to file: 
```sh
gpg --output snap_dec2 --decrypt snap.tar.gz.
```

---


## Export Keys

Export public key: 
```sh 
gpg --export --armor self@self.com > pubkey.asc
```
Export Private key: 
```sh
gpg --export-secret-keys --armor self@self.com > privkey.asc
```

tar the keys: 
```sh
tar -czvf gpg_folder.tar.gz gpg_folder
```

backup in cloud. 

to import these keys on a new device or secure server:

```sh
gpg --import pubkey.asc
gpg --import privkey.asc
```

next, trust keys: 

```bash
gpg --edit-key self@self.com
	trust
	5
	quit
```

---

## Encrypt Passwords using `pass`

About Pass: Stores, retrieves, generates, and synchronizes passwords securely

```sh
yay -S extra/pass
pass init username@email.com
pass insert outlook
```

Generate complex passwords: `pass generate genrate-example 30`

`pass insert work/company1/email`
