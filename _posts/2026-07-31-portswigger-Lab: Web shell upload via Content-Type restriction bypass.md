---
layout: post
title: "Lab: Web shell upload via Content-Type restriction bypass [Write-up]"
date: 2026-07-31
platform: portswigger
vulntype: file_uploads_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Web shell upload via Content-Type restriction bypass

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab contains a vulnerable image upload function. It attempts to prevent users from uploading unexpected file types, but relies on checking user-controllable input to verify this.

To solve the lab, upload a basic PHP web shell and use it to exfiltrate the contents of the file `/home/carlos/secret`. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

**Step 1**

Log in to our account 

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod1.png" alt="file_uploads_vuln" />
</p>

**Step 2**

Just like in the previous lab, let's try to load the same exploit 

```python
nano exploit.php
```

```python
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod5.png" alt="file_uploads_vuln" />
</p>

But we're getting an error message that says the app only accepts images

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod6.png" alt="file_uploads_vuln" />
</p>

**Step 3**

In the lab header, we can see that we need to change the `Content-Type`. Let's send a photo and take a look at this parameter.

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod7.png" alt="file_uploads_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod8.png" alt="file_uploads_vuln" />
</p>

Let's copy this parameter and try changing it in our exploit's query 

**Step 4**

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod9.png" alt="file_uploads_vuln" />
</p>

and yes, we were able to upload the file to the server 

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod10.png" alt="file_uploads_vuln" />
</p>

**Step 5**

Let's go to our profile and intercept the request 

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod11.png" alt="file_uploads_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod12.png" alt="file_uploads_vuln" />
</p>

We were able to retrieve the contents of the file, and now we can send it back in our response and complete the lab.

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
