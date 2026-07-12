---
layout: post
title: "Broken brute-force protection, IP block [Write-up]"
date: 2026-07-12
platform: portswigger
vulntype: authentication
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Broken brute-force protection, IP block

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab is vulnerable due to a logic flaw in its password brute-force protection. To solve the lab, brute-force the victim's password, then log in and access their account page.

Your credentials: wiener:peter

Victim's username: carlos

[Candidate passwords](https://portswigger.net/web-security/authentication/auth-lab-passwords)

## Solution

In this problem, we need to perform a brute-force attack on the “carlos” user account. We have the password for the “wiener” account.

**Step 1: Vulnerability Search**

If we try to send three invalid requests, the limit will kick in, and we'll have to wait one minute before trying again. 

<p align="center">
  <img src="/assets/css/img/Authentication/Auth22.png" alt="Authentication" />
</p>

The vulnerability lies in the fact that, upon successful login, request attempts are reset. This means that if we send two invalid requests followed by one successful one, the limit is reset, and we can send two more invalid requests—which is exactly what we’ll take advantage of. 

We need to modify our dictionaries so that we don't run into any limitations, so we'll set up the dictionary as follows: 

More specifically, here's what the Python code looks like that can create a dictionary like this for us 

```python
with open("username.txt", "w") as f:
    for _ in range(100):
        f.write("wiener\n")
        f.write("carlos\n")
```

For passwords, the dictionary will look like this:

```python
with open("password.txt", "r", encoding="utf-8") as f:
    lines = f.readlines()

with open("password.txt", "w", encoding="utf-8") as f:
    for line in lines:
        f.write("Green\n")
        f.write(line)
```

Now that we have the dictionaries ready, we can launch the attack by going to Burp Intruder `CTRL + i`

Switch the mode from “Sniper” to “Pitchfork Attack,” enter a dictionary containing our usernames for the ‘username’ field, enter a second dictionary containing our passwords for the “password” field, and launch the attack. 

<p align="center">
  <img src="/assets/css/img/Authentication/Auth23.png" alt="Authentication" />
</p>

After waiting a moment, we can see a response with code 302 for the user “carlos”—this is the correct password. Enter it into the login form and complete the lab. 

