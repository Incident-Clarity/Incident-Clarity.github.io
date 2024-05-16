---
title: "Zoxide: A smart `cd` tool"
date: 2024-04-22T21:06:35+05:30
url: /zoxide-a-cd-replacment/
categories:
    - Linux
    - Windows
#  - Networking
#  - Security
#  - CTF
#  - Intervew
#  - Course
tags:
    - Zoxide
    - linux
    - tool
    - terminal
    - windows
    - mac
#  - Ubuntu
#  - SIEM
draft: true
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
showSummary: false
summary: ""
---
<!--more-->

**Todo:**
- edit later :)
- add examples
- explain initialization steps

Recently I came accross this utility called as Zoxide through github. They claimed to be, "A smarter cd command for your terminal". So, I gave it a try. 

It utilizes rust and fzf for blazingly fast directory triversal. 

In simple words, this utility makes a database of all the directories you visit to and when you call it, it exactly knows which one to jump into using just small words in the directory. I will show some examples to demnstrate the same.

## 

