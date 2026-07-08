---
layout: post
title: "Lab: Password reset broken logic [Write-up]"
date: 2026-07-08
platform: portswigger
vulntype: authentication
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Password reset broken logic

**Platform:** PortSwigger

**Difficulty:** Easy  

---

# Description 

This lab's password reset functionality is vulnerable. To solve the lab, reset Carlos's password then log in and access his "My account" page.

Your credentials: `wiener:peter`

Victim's username: `carlos`

## Solution

In this problem, we need to change the password for the user “carlos” without knowing his password or having access to his email, by exploiting a misconfigured server logic.

For this task, we'll need Burp Suite, which we'll use to intercept the request and modify it.

Since our vulnerability is related to password resets, let's start with that right away. If we try to reset the password for the user “carlos” right away, we won't be able to do anything—we don't have access to his email. So let's try resetting the password for the user “wiener” first. 


Click the “Reset Password” button and enter your username or email address

<p align="center">
  <img src="/assets/css/img/Authentication/Auth5.png" alt="Authentication" />
</p>

We received an email with a link to reset our password 

<p align="center">
  <img src="/assets/css/img/Authentication/Auth6.png" alt="Authentication" />
</p>

Let's change the password to, say, 12345, and then intercept the request using Burp Suite. 

<p align="center">
  <img src="/assets/css/img/Authentication/Auth7.png" alt="Authentication" />
</p>

This is what our request looks like. There's a very interesting detail in it—namely, the “username” parameter, which specifies our name. Let's try changing it to “carlos.”

<p align="center">
  <img src="/assets/css/img/Authentication/Auth8.png" alt="Authentication" />
</p>

We are sending this request

And let's try logging back into the “carlos” account 

We actually managed to log in to the “carlos” user account and complete the lab. 
