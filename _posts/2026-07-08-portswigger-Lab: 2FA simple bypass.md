---
layout: post
title: "2FA simple bypass [Write-up]"
date: 2026-07-08
platform: portswigger
vulntype: authentication
difficulty: Easy
category: write-up
tags: [authentication, web]
---

## Overview

**Lab:** 2FA simple bypass

**Platform:** PortSwigger

**Difficulty:** Easy  

---

# Description 

This lab's two-factor authentication can be bypassed. You have already obtained a valid username and password, but do not have access to the user's 2FA verification code. To solve the lab, access Carlos's account page.

Your credentials: `wiener:peter`

Victim's credentials: `carlos:montoya`

## Solution

This is a really interesting challenge, but at the same time, it’s ridiculously simple. We have the credentials for two accounts. Our task is to log in to the “carlos” account by bypassing two-factor authentication. We were able to log in to our own account using our email address—we received a code via email and successfully logged in. Now let’s try to log in to the “carlos” account.


After logging in with the correct credentials for user **Carlos**, the application redirected the user to the `/login2` page to enter the two-factor authentication code. However, the server had already created a session after verifying the username and password and did not require confirmation of 2FA completion when accessing a secure page. By changing the URL from `/login2` to `/my-account?id=carlos`, it was possible to access the account without entering the second factor. The vulnerability lies in the fact that the server did not verify that the two-factor authentication step had been successfully completed, but relied solely on the existence of a partially authenticated session.


