---
title: "Linux File Permissions"
date: 2025-02-02T16:23:22+05:30
url: /Linux-file-perm/
categories:
  - Linux
#  - Windows
#  - Networking
  - Security
#  - CTF
#  - Intervew
  - Courses
#  - Notes
tags:
#  - Ubuntu
#  - SIEM
  - GoogleCybersecurityProgram 
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
summary: "Showcased the practical application of Linux commands to manage file permissions securely."
---

## Project Description

This project demonstrates practical Linux administration skills by using core commands like `ls` and [`chmod`](https://man7.org/linux/man-pages/man2/chmod.2.html) to manage and understand POSIX permissions.

## Step 1: Checking File and Directory Permissions
To check existing file permissions of the projects directory:

```bash
ls -la /home/user91/projects/
```
**Sample output:**
```
drwxr-xr-x 4 user91 researchers 4096 Feb 2 14:30 .
drwxr-xr-x 3 root        root        4096 Feb 10 14:30 ..
-rw-rw-r-- 1 user91 researchers 1024 Feb 2 14:30 project_a.md
-rw-r--r-- 1 user91 researchers 2048 Feb 2 14:30 .project_x.txt
drwxrwxr-x 2 user91 researchers 4096 Feb 2 14:30 drafts
```

### Understanding the 10-Character Permission Strings

**10-character string representation:** `drw-rw-r-x`

1. **First character:** File type (`-` = regular file, `d` = directory)
2. **Characters 2-4 (next 3):** *User* permissions (read, write, execute).
3. **Characters 5-7 (next 3):** *Group* permissions (read, write, execute).
4. **Characters 8-10 (last 3):** *Others* permissions (read, write, execute)


**Example:** `-rw-r--r--`
- `-` indicates a regular file.
- `rw-` means the user can read and write.
- `r--` means the group can only read.
- `r--` means others can only read.



## Step 2: Modifying File Permissions Using chmod

### A. Removing Unauthorized Write Access

**Problem:** `project_a.md` has unsafe permission `rw-rw-rw-` (Group and others have write access to the file).

**Solution:** 

```bash
chmod 644 /home/user91/project_a.md
```

This command sets the permissions to:
- **User:** Read and write (6 = 4 + 2)
- **Group:** Read only (4)
- **Others:** Read only (4)

After running the command, the file's permissions were confirmed with:

```bash
ls -la /home/user91/project_a.md
```

**Result:**

```
-rw-r--r-- 1 user91 researchers 1024 Feb 2 14:30 project_a.md
```

### B. Adjusting Permissions for a Hidden File
**Problem:** The research team archived the hidden file `.project_x.txt`. It should have no write permissions, with read permissions granted to the user and group. I used the following command:


**Solution:**
```bash
chmod u=r,g=r,o= /home/user91/.project_x.txt
```

This command updates the permissions as follows:
- **Owner:** Read only (4)
- **Group:** Read only (4)
- **Others:** No permissions (0)

**Verify Permissions:**

```bash
ls -la /home/user91/.project_x.txt
```

**Result:**

```
-r--r----- 1 user91 researchers 2048 Feb 2 14:30 .project_x.txt
```

### C. Restricting Directory Access

**Requirement:** Only the user `user91` should access the `drafts` directory.

**Solution:**
```bash
chmod 700 drafts/
```

This command ensures that:
- **User (user91):** Full access (read, write, execute)
- **Group & Others:** No access

**Verifying the change:**

```bash
ls -la /path/to/drafts
```

**Result:**
```
drwx------ 2 user91 researchers 4096 Feb 2 14:30 drafts
```

## Summary
This project showcased the practical application of Linux commands to manage file permissions securely. 

Which included: 
1. Permission auditing using `ls -la`
2. Removing group write access from project files
3. Securing archived documents with proper hidden file permissions
4. Implementing strict directory access controls

This exercise not only reinforced my command-line skills but also highlighted the importance of proper authorization and principle of least privilege in maintaining system security.