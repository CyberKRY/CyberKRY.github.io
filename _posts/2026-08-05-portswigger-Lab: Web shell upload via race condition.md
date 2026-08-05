---
layout: post
title: "Lab: Web shell upload via race condition [Write-up]"
date: 2026-08-05
platform: portswigger
vulntype: file_uploads_vuln
difficulty: Hard
category: write-up
tags: [web]
---

## Overview

**Lab:** Web shell upload via race condition

**Platform:** PortSwigger

**Difficulty:** Hard

---

## Description


This lab contains a vulnerable image upload function. Although it performs robust validation on any files that are uploaded, it is possible to bypass this validation entirely by exploiting a race condition in the way it processes them.

To solve the lab, upload a basic PHP web shell, then use it to exfiltrate the contents of the file /home/carlos/secret. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

This lab is vulnerable to web shell injection via a race condition.

**Step 1**

Let's log in to our account 

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod1.png" alt="file_uploads_vuln" />
</p>

**Step 2**

Based on the theory 

**Race Condition** is a vulnerability that occurs when two or more requests access the same resource simultaneously without proper synchronization. This can allow an attacker to bypass restrictions, perform the same action multiple times, or cause the application to enter an unintended state.

We'll carry out our attack—let's start by intercepting the request to send the malicious code. 

The payload looks like this

```python
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

Let's use Burp Suite to intercept this POST request 

Now let's try to make a GET request for this file by changing the URL to 

```python
https://0af400b50467ccf580e98fac00580029.web-security-academy.net/files/avatars/shell.php
```

And let's intercept this request 

Send these two requests to the Repeater and add them to a single group, then select “Send Group (parallel)” and send the requests until we receive a response. 

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod18.png" alt="file_uploads_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod19.png" alt="file_uploads_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod20.png" alt="file_uploads_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod21.png" alt="file_uploads_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod22.png" alt="file_uploads_vuln" />
</p>

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>

Once we've obtained the contents of the secret file, we enter the answer and complete the lab.


