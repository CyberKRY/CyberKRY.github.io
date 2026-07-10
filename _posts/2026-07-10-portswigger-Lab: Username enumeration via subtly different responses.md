---
layout: post
title: "Username enumeration via subtly different responses [Write-up]"
date: 2026-07-10
platform: portswigger
vulntype: authentication
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Username enumeration via subtly different responses

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab is subtly vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:

[Candidate usernames](https://portswigger.net/web-security/authentication/auth-lab-usernames)

[Candidate passwords](https://portswigger.net/web-security/authentication/auth-lab-passwords)

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

## Solution

The goal is to carry out a brute-force attack. Our objective is to find the right user; to do so, we need to identify differences in the responses from the server. 

To solve this problem, I'll use Burp Suite; first, I'll intercept my POST request 

Replace the “username” field with special characters and load the dictionary provided by the lab.

<p align="center">
  <img src="/assets/css/img/Authentication/Auth9.png" alt="Authentication" />
</p>

<p align="center">
  <img src="/assets/css/img/Authentication/Auth10.png" alt="Authentication" />
</p>

Let's run it and see the answers 

After a while, we may notice that there is no obvious information that would immediately tell us which user is in the system; the only difference is in size. 

<p align="center">
  <img src="/assets/css/img/Authentication/Auth11.png" alt="Authentication" />
</p>

> P.S. I tried comparing two queries of the same size, and the only difference between them is the identifier, but that doesn't tell us anything useful.

The next thing I tried was comparing the error messages in the responses between different usernames. I used “Invalid username or password.” as my basis.

To perform this test, we will use Intruder with a filter 

First, click the Settings tab, find “Grep - Match,” click ‘Clear’ if it's already filled in, add the error “Invalid username or password.” there, and launch the attack. 

<p align="center">
  <img src="/assets/css/img/Authentication/Auth12.png" alt="Authentication" />
</p>

<p align="center">
  <img src="/assets/css/img/Authentication/Auth13.png" alt="Authentication" />
</p>

We can see that one query is different from the rest—specifically, it lacks a period at the end. This error is what gave away our user. 

<p align="center">
  <img src="/assets/css/img/Authentication/Auth14.png" alt="Authentication" />
</p>

Now we can safely launch a brute-force attack: we enter the username we found into the username field and special characters into the password field, load the password dictionary provided to us, and launch the attack.

And we find 1 response with code 302, which is the correct password 
