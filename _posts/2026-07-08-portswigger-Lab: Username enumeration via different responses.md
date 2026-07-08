---
layout: post
title: "Username enumeration via different responses [Write-up]"
date: 2026-07-08
platform: portswigger
vulntype: authentication
difficulty: Easy
category: write-up
tags: [authentication, web]
---

## Overview

**Lab:** Username enumeration via different responses

**Platform:** PortSwigger

**Difficulty:** Easy  

---

# Description 

This lab is vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:

[Candidate usernames](https://portswigger.net/web-security/authentication/auth-lab-usernames)

[Candidate passwords](https://portswigger.net/web-security/authentication/auth-lab-passwords)

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

## Solution

In this problem, we are given two files, `usernames` and `passwords`. Our task is to perform a brute-force attack. The best—and, in my opinion, the fastest—way to do this is to use a Sniper Attack. Why? Because trying all 10,000 combinations would take too long. Instead, we can simply find out which user is in the system and then target that user.

Intercept the request using Burp Suite, then use the CTRL + i shortcut to send the intercepted request to the Intruder tab. Next, enter special characters in the “username” field and leave a simple password in the “password” field.

<p align="center">
  <img src="/assets/css/img/Authentication/Auth1.png" alt="Authentication" />
</p>

<p align="center">
  <img src="/assets/css/img/Authentication/Auth2.png" alt="Authentication" />
</p>

We launch Intruder and wait until we find our user. After a while, the only request where the response contains an “incorrect password” error is for the user we're looking for. Now that we know this, we can target that account and start a brute-force attack.

<p align="center">
  <img src="/assets/css/img/Authentication/Auth3.png" alt="Authentication" />
</p>

Enter the username in the “username” field, enter special characters in the “password” field, load the dictionary, launch the attack, and wait for a response. 

After a while, we see a single request with a 302 response code—that is the correct password.

<p align="center">
  <img src="/assets/css/img/Authentication/Auth4.png" alt="Authentication" />
</p>

Enter the correct information into the login form and complete the lab. 
