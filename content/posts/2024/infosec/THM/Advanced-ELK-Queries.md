---
title: "Advanced ELK Queries"
date: 2024-05-25T20:21:07+05:30
url: /Advanced-ELK-Queries/
categories:
#  - Linux
#  - Windows
#  - Networking
#  - Security
#  - CTF
#  - Intervew
#  - Courses
#  - Notes
tags:
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

> Aayan Ta | 25th May, 2024 | 20:21 | [Advanced ELK Queries](https://tryhackme.com/r/room/advancedelkqueries)
## Advanced ELK Queries

**Learning Objective**

- Learn about different advanced queries
- Learning about different query syntaxes (Kibana Query Language and Lucene)

### Task 1: A Primer on Advanced Queries

- **Syntaxes**
  - Kibana supports two query languages: KQL (Kibana Query Language) and Lucene Query Syntax.
  - **KQL**: User-friendly, provides autocomplete suggestions, supports various operators and functions.
  - **Lucene**: More powerful, harder to learn, used in Elasticsearch and other search engines.

- **Special Characters**
  - Reserved characters in ELK queries: +, -, =, &&, ||, &, |, !.
  - To use reserved characters, precede them with a backslash (e.g., `username:User\+1`).

- **Wildcards**
  - `*`: Matches any number of characters (e.g., `product_name:monit*`).
  - `?`: Matches a single character (e.g., `name:J?n`).

**Questions:**
1. How do you escape the text "password:Me&Try=Hack!" (Not including the double quotes)
	- `password:Me\&Try\=Hack\!`

2. Using wildcards, what will your query be if you want to search for all documents that contain the words "hacking" and "hack" in the "activity" field?
	- `activity:hack*`

### Task 2: 



### Task 3: 



### Task 4: 



### Task 5: 



### Task 6: 




