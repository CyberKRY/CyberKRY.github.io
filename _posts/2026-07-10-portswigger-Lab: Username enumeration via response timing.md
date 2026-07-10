---
layout: post
title: "Username enumeration via response timing [Write-up]"
date: 2026-07-10
platform: portswigger
vulntype: authentication
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Username enumeration via response timing

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab is vulnerable to username enumeration using its response times. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

Your credentials: `wiener:peter`

Candidate [usernames](https://portswigger.net/web-security/authentication/auth-lab-usernames)

Candidate [passwords](https://portswigger.net/web-security/authentication/auth-lab-passwords)

 > Hint: To add to the challenge, the lab also implements a form of IP-based brute-force protection. However, this can be easily bypassed by manipulating HTTP request headers.

## Solution

In this lab assignment, we need to identify a user in the system based on a timestamp. The vulnerability itself lies in the fact that the server performs a password hash check for an existing user. It is this additional operation that causes a noticeable delay.

> ! The steps described in this article are not the fastest in real-world conditions 

Well, let's start by intercepting the request using Burp Suite. The hint told us that the server blocks IP addresses if there are too many attempts from them, but this can be bypassed by adding an X-Forwarded-For header.

`X-Forwarded-For (XFF)` -  request header is a de-facto standard header for identifying the originating IP address of a client connecting to a web server through a proxy server

**Step 1: Intercept the Request**

<p align="center">
  <img src="/assets/css/img/Authentication/Auth15.png" alt="Authentication" />
</p>

**Step 2: Add a title and special characters**

We need to add special characters to the X-Forwarded-For and username fields

<p align="center">
  <img src="/assets/css/img/Authentication/Auth16.png" alt="Authentication" />
</p>

**Step 3: Switch the Intruder attack to Pitchfork attack mode**

<p align="center">
  <img src="/assets/css/img/Authentication/Auth17.png" alt="Authentication" />
</p>

**Step 4: Add dictionaries**

For the X-Forwarded-For header, you can use any words—it doesn't matter—as long as they aren't repeated too many times. For example, I used a list of animal names. 

The lab provided a list of words for the “username” field -> Candidate [usernames](https://portswigger.net/web-security/authentication/auth-lab-usernames)

<p align="center">
  <img src="/assets/css/img/Authentication/Auth18.png" alt="Authentication" />
</p>

<p align="center">
  <img src="/assets/css/img/Authentication/Auth19.png" alt="Authentication" />
</p>

**Step 5: Password**

This is a very important step. It’s important to enter the password correctly so that it’s easier to tell which request will take a long time to process—the longer the password, the more clearly you’ll see the pause. 

So I came up with this password

<p align="center">
  <img src="/assets/css/img/Authentication/Auth20.png" alt="Authentication" />
</p>

We can launch an attack and monitor which request takes the longest to process 

> P.S. This method isn't exactly convenient in a real-world setting—you might end up with a long list of users, and this method isn't the most convenient—but within the context of this lab, where the list of users isn't very long, we don't need to worry too much about it. 

Well, as far as I could tell, the user whose profile took the longest to load was antivirus.

Now it will be easy for us to perform a brute-force attack on this user's passwords

**Step 6: Brute-Force Password Attack**

In the “username” field, enter our username, and in the “password” field, enter special characters. Now let's use the dictionary from the lab. -> Candidate [passwords](https://portswigger.net/web-security/authentication/auth-lab-passwords)

<p align="center">
  <img src="/assets/css/img/Authentication/Auth21.png" alt="Authentication" />
</p>

We launch the attack, and after a while we see a response with a 302 status code, which confirms the password. We log in using these credentials and complete the lab. 
