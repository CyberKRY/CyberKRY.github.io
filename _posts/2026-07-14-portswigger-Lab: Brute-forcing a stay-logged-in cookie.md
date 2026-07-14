---
layout: post
title: "Lab: Brute-forcing a stay-logged-in cookie [Write-up]"
date: 2026-07-14
platform: portswigger
vulntype: authentication
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Brute-forcing a stay-logged-in cookie

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab allows users to stay logged in even after they close their browser session. The cookie used to provide this functionality is vulnerable to brute-forcing.

To solve the lab, brute-force Carlos's cookie to gain access to his My account page.

Your credentials: `wiener:peter`

Victim's username: `carlos`

[Candidate passwords](https://portswigger.net/web-security/authentication/auth-lab-passwords)

## Solution

## Step 1 

Let's start by logging into our “wiener” account. In the login form, we can see the “Stay logged in” checkbox. The task description states that the vulnerability lies in this setting, so we'll check the box and log into our account. 

## Step 2 

Let's intercept the request by refreshing the page, and we'll notice a detail like “stay-logged-in.” This cookie ensures that the user remains logged in even after closing the browser.

<p align="center">
  <img src="/assets/css/img/Authentication/Auth27.png" alt="Authentication" />
</p>


Let's use Burp Suite to highlight this cookie, and we can see that its structure is base64(username:MD5).

<p align="center">
  <img src="/assets/css/img/Authentication/Auth28.png" alt="Authentication" />
</p>


Since we need to perform a brute-force attack, we need to convert our dictionary into this format. We're targeting the user “carlos,” so we'll create a script that generates a dictionary of the form base64(carlos:MD5(password)).

```python
import hashlib
import base64

PREFIX = "carlos:"
INPUT_FILE = "passwords.txt"
OUTPUT_FILE = "output.txt"

with open(INPUT_FILE, "r", encoding="utf-8", errors="ignore") as infile, \
     open(OUTPUT_FILE, "w", encoding="utf-8") as outfile:

    for line in infile:
        password = line.strip()
        if not password:
            continue

        md5_hash = hashlib.md5(password.encode("utf-8")).hexdigest()

        text = f"{PREFIX}{md5_hash}"

        b64 = base64.b64encode(text.encode("utf-8")).decode("utf-8")

        outfile.write(b64 + "\n")

print(f"File ready: {OUTPUT_FILE}")
```

The result will be a dictionary containing pre-generated “tay-logged-in” cookies. The next step is to send our request to Intruder, replace “tay-logged-in” with special characters, and don't forget to replace “/my-account?id=carlos.” Select the “sniper” attack type, insert our dictionary, and launch the attack.

Your request should look like this

<p align="center">
  <img src="/assets/css/img/Authentication/Auth29.png" alt="Authentication" />
</p>


<p align="center">
  <img src="/assets/css/img/Authentication/Auth30.png" alt="Authentication" />
</p>


After waiting a moment, you'll see a response with a 200 status code, and you've successfully solved the problem. 

<p align="center">
  <img src="/assets/css/img/Authentication/Auth31.png" alt="Authentication" />
</p>


