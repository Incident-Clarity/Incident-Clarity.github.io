---
title: "[THM] Attacktive Directory"
date: 2024-04-19T08:44:16+05:30
url: /attacktivedirectory-CTF/
categories:
  - Security
  - ctfs
  - Windows
  - networking
tags:
  - Windows
  - ActiveDirectory
  - tryhackme
draft: false
author: aayan
showauthor: false
showDateOnlyInArticle: false
showDateUpdated: true
showHeadingAnchors: false
showPagination: false
showReadingTime: true
showTableOfContents: true
showTaxonomies: true
showWordCount: true
sharingLinks: false
showEdit: false
showViews: false
showLikes: false
showSummary: true
summary: 99% of Corporate networks run off of AD. But can you exploit a vulnerable Domain Controller?
---
<!--more-->

(to do)

## Attacktive Directory CTF

**Description: 99% of Corporate networks run off of AD. But can you exploit a vulnerable Domain Controller?**


### Task 1: Deploy The Machine
---

Target IP = 10.10.178.210


### Task 2: Setup
---

**Installing Impacket**


```sh
yay -S blackarch/impacket-ba
```

or

```sh
sudo git clone https://github.com/SecureAuthCorp/impacket.git /opt/impacket
sudo pip3 install -r /opt/impacket/requirements.txt
cd /opt/impacket/ 
sudo pip3 install .
sudo python3 setup.py install
```

**Check if impacket is installed correctly:**
`psexec.py -h`


**Install Bloodhound and Neo4j**

```sh
yay -S bloodhound
```


### Task 3: Welcome to Attacktive Directory 
---

#### Enumeration (nmap)

```bash
sudo nmap -v -sC -sV -oA nmap/attacktivedirectory 10.10.178.210
```

```sh
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2024-04-19 03:51:37Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
3389/tcp open  ms-wbt-server syn-ack ttl 127 Microsoft Terminal Services
| ssl-cert: Subject: commonName=AttacktiveDirectory.spookysec.local
| Issuer: commonName=AttacktiveDirectory.spookysec.local
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-04-18T03:22:11
| Not valid after:  2024-10-18T03:22:11
| MD5:   6859:fc45:c4bc:9629:3ae8:611a:7b4d:9ea8
| SHA-1: 81c0:e3c6:0144:716e:5931:5538:230e:9424:45b2:968a
| -----BEGIN CERTIFICATE-----
| MIIDCjCCAfKgAwIBAgIQRZYdLn3NkIhJZt7e8/qUDjANBgkqhkiG9w0BAQsFADAu
| MSwwKgYDVQQDEyNBdHRhY2t0aXZlRGlyZWN0b3J5LnNwb29reXNlYy5sb2NhbDAe
| Fw0yNDA0MTgwMzIyMTFaFw0yNDEwMTgwMzIyMTFaMC4xLDAqBgNVBAMTI0F0dGFj
| a3RpdmVEaXJlY3Rvcnkuc3Bvb2t5c2VjLmxvY2FsMIIBIjANBgkqhkiG9w0BAQEF
| AAOCAQ8AMIIBCgKCAQEAyKFhf9VzLoY8SL9d2gf4JG9kYlq6VEBliNiii8GP7NFk
| 2ciOXHBl9aKdxazPx2FUs/0ZgPdiFaUhs1ZMbgB32jWPeMzXiFkwTOh/UhpgNykG
| LQQ++x13B15QhfftCiUsFs/WZU1Hh/Nh+h8o/u2T02C2x8cu5i9o4Fv8Zk7s/78D
| 6fFDWBETHim62AnYqYY0OS8hXlbt68dTKx4cJBwXIk0iFDSvgm4xj18OqJY5rzTf
| DgeeKXgvRKOq3OVmsEELsUY73fAumfGcpZ3Y95ms/AmXtbibU4Gi+A9OXJTZ29gm
| e1Oa4LxzVdxQZ3Pb7enIKeU6ug/5VNh9JUm32CC/FQIDAQABoyQwIjATBgNVHSUE
| DDAKBggrBgEFBQcDATALBgNVHQ8EBAMCBDAwDQYJKoZIhvcNAQELBQADggEBADA0
| iZ5GPmKnQJvoOPdHBChkeVIWp1HMrjpsbqioJdEr6hsbocCDWrTzDr161XUKMBzl
| zjG30yzLVFFI6GK6xeUvGQkB1kQBJgpYchs5ld9S9SSJSI6B72f+K3+Lfc6JOQDh
| srej7EcFNIwcliUF6DL2SehATa1xbOigQ1whQojQxcBk4CwEBUfLPLgogcxS9w4o
| wYfQWKa/n7wUn87eL44Bi6yhPVLoJQ/KG7aBN+NeSPy5RxZ0eeCeGSb8sXjJqbZh
| kFyeKwT44KmvT3MrRjq/KaxrTMPHS9DVxlvWTSvoSfrxhXBWPVRmWwmVpBMtJ+3J
| RDHIhMTIRDmU6X8uj4g=
|_-----END CERTIFICATE-----
| rdp-ntlm-info: 
|   Target_Name: THM-AD
|   NetBIOS_Domain_Name: THM-AD
|   NetBIOS_Computer_Name: ATTACKTIVEDIREC
|   DNS_Domain_Name: spookysec.local
|   DNS_Computer_Name: AttacktiveDirectory.spookysec.local
|   Product_Version: 10.0.17763
|_  System_Time: 2024-04-19T03:51:49+00:00
|_ssl-date: 2024-04-19T03:51:57+00:00; -1s from scanner time.
Service Info: Host: ATTACKTIVEDIREC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: 0s, deviation: 0s, median: -1s
| smb2-time: 
|   date: 2024-04-19T03:51:49
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 51640/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 21261/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 20838/udp): CLEAN (Timeout)
|   Check 4 (port 29234/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
```


> Got 15 open ports using the all port scan

```bash
sudo nmap -v -sC -sV -p- -oA nmap/attacktivedirectory-all 10.10.178.210
```


**openports:**

```sh
less nmap/attacktivedirectory-all.nmap | grep open
53/tcp   open  domain        Simple DNS Plus
80/tcp   open  http          Microsoft IIS httpd 10.0
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-04-19 04:08:13Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
3389/tcp open  ms-wbt-server Microsoft Terminal Services
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
9389/tcp open  mc-nmf        .NET Message Framing
```

**Adding to /etc/hosts**

---

```sh
sudo vim /etc/hosts
```

```sh
10.10.178.210   AttacktiveDirectory.spookysec.local     spookysec.local
```

**Notes**: Flags for each user account are available for submission. You can retrieve the flags for user accounts via RDP (Note: the login format is spookysec.local\User at the Window's login prompt) and Administrator via Evil-WinRM.

Questions:
1. What tool will allow us to enumerate port 139/445?
  - SMB ports 139 and 445
  - `enum4linux`

2. What is the NetBIOS-Domain Name of the machine?
  - `THM-AD`


3. What invalid TLD do people commonly use for their Active Directory Domain?
	- spookysec.local
  - `.local`



### Task 4: Enumerating Users via Kerberos
---

- **Kerberos** is a key authentication service within Active Directory. 
- With this port open, we can use a tool called [Kerbrute](https://github.com/ropnop/kerbrute/releases) (by Ronnie Flathers @ropnop) to brute force discovery of users, passwords and even password spray!

or,

```sh
yay -S blackarch/kerbrute
```

**Enumeration:**

For this box, a modified [User List](https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/userlist.txt) and [Password List](https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/passwordlist.txt) will be used to cut down on time of enumeration of users and password hash cracking. 

It is **NOT** recommended to brute force credentials due to account lockout policies that we cannot enumerate on the domain controller.

**Here is what we do:**

- `wget` the usernames and passwords

- `kerbrute -h` 

- I did not know how to use this tool or how domain controlsers work.


- `kerbrute userenum --dc "spookysec.local" -d "spookysec.local" -v -o kerb_userenum userlist.txt | grep "VALID USERNAME"`

**got these valid users**:
```sh
2024/04/19 10:15:18 >  [+] VALID USERNAME:       james@spookysec.local
2024/04/19 10:15:22 >  [+] VALID USERNAME:       svc-admin@spookysec.local
2024/04/19 10:15:26 >  [+] VALID USERNAME:       James@spookysec.local
2024/04/19 10:15:27 >  [+] VALID USERNAME:       robin@spookysec.local
2024/04/19 10:15:39 >  [+] VALID USERNAME:       darkstar@spookysec.local
2024/04/19 10:15:49 >  [+] VALID USERNAME:       administrator@spookysec.local
2024/04/19 10:16:09 >  [+] VALID USERNAME:       backup@spookysec.local
2024/04/19 10:16:16 >  [+] VALID USERNAME:       paradox@spookysec.local
2024/04/19 10:17:15 >  [+] VALID USERNAME:       JAMES@spookysec.local
2024/04/19 10:17:31 >  [+] VALID USERNAME:       Robin@spookysec.local
```




### Task 5: Abusing Kerberos
---




### Task 6: Back to the Basics
---




### Task 7: Elevating Privileges within the Domain
---




### Task 8: Flag Submission Panel
---




