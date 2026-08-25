---
layout: post
title: "Lab: SameSite Lax bypass via method override [Write-up]"
date: 2026-08-25
platform: portswigger
vulntype: csrf
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** SameSite Lax bypass via method override

**Platform:** PortSwigger

**Difficulty:** Medium

---

## Description

This lab's change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

Let's start with the theory: what is SameSite and what parameters does it have?

`SameSite` -  is a cookie attribute that controls whether a cookie is sent with cross-site requests. It has three main values: Strict, Lax, and None. Strict provides the strongest protection, Lax allows cookies in some cross-site requests, and None allows cross-site cookies but requires Secure.

## Step 1 

Log in to our account

`wiener:peter`

## Step 2

The problem description states that the vulnerability lies in changing the email address, so let's intercept the request to change the email address using Burp Suite.

<p align="center">
  <img src="/assets/css/img/CSRF/csrf12.png" alt="csrf" />
</p>

## Step 3 

We'll pass this request to Reapeter and create a payload. There are several ways to do this; if you have the Pro version of Burp Suite, you can do it within the tool. If you have the standard version, you can refer to my solution for the first lab and use this [CSRF payload generator](https://cyberkry.github.io/write-up/portswigger-Lab-CSRF-vulnerability-with-no-defenses/).

<p align="center">
  <img src="/assets/css/img/CSRF/csrf13.png" alt="csrf" />
</p>

## Step 4

Let's go to the exploit service and insert our payload into the body. I recommend removing any unnecessary details so the payload is cleaner. 

<p align="center">
  <img src="/assets/css/img/CSRF/csrf14.png" alt="csrf" />
</p>

## Step 5 

Let's now finish setting up our payload. I used this article on [method spoofing](https://codeigniter4.github.io/userguide/incoming/methodspoofing.html). In short, the application uses `SameSite=Lax` by default. This usually prevents session cookies from being sent with cross-site, state-changing requests, such as POST requests. However, `.` SameSite=Lax allows cookies to be sent with cross-site GET requests. The application also supported HTTP method overriding via `_method`, so I was able to send a cross-site GET request containing `_method=POST.`. The browser processed the request as a GET and included the session cookie, while the application subsequently interpreted the request as a POST and performed a state-changing action.

Ultimately, our payload looks like this 

```python
<form method="GET" action="https://ID.web-security-academy.net/my-account/change-email">
        <input type="hidden" name="_method" value="POST">
	<input type="hidden" name="email" value="test2@test.com"/>
	<input type="submit" value="Submit">
</form>
<script>
        document.forms[0].submit();
</script>
```

<p align="center">
  <img src="/assets/css/img/CSRF/csrf15.png" alt="csrf" />
</p>

Click “Deliver exploit to victim” and complete the lab

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
