---
layout: post
title: "Lab: OS command injection, simple case [Write-up]"
date: 2026-07-18
platform: portswigger
vulntype: command_injection
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** OS command injection, simple case

**Platform:** PortSwigger

**Difficulty:** Easy

---


## Description 

This lab contains an OS command injection vulnerability in the product stock checker.

The application executes a shell command containing user-supplied product and store IDs, and returns the raw output from the command in its response.

To solve the lab, execute the `whoami` command to determine the name of the current user.

## Solution

In this exercise, we need to execute the `whoami` command and determine the user by exploiting a command injection vulnerability. 

The first thing we'll do is identify the location of the vulnerability. The problem description tells us that it's the section containing product and store IDs. 

<p align="center">
  <img src="assets/css/img/Command injection/Comnd_inj1.png" alt="Command_injection" />
</p>

<p align="center">
  <img src="assets/css/img/Command injection/Comnd_inj2.png" alt="Command_injection" />
</p>

On the page, there's a button and a city selector. If we click “Check Stock,” it will show us how many products are in stock. Let's intercept this request using Burp Suite. 

Based on the vulnerability theory, we need to inject our own string using special characters 

> OS Command Injection allows an attacker to inject operating system commands into an application. This happens when user input is included in a system command without proper validation, allowing arbitrary commands to be executed on the server.

There are several ways to solve this problem, but I'll explain the simplest one.

```python
productId=1&storeId=1|whoami
```

<p align="center">
  <img src="assets/css/img/Command injection/Comnd_inj3.png" alt="Command_injection" />
</p>

<p align="center">
  <img src="assets/css/img/Command injection/Comnd_inj4.png" alt="Command_injection" />
</p>

It's important to use the | symbol here; if we try anything else, the command won't run. There's another way to do it, and it looks like this: 

```python
productId=1& whoami & #&storeId=1

URL encoding:

productId=1%26+whoami+%26+%23&storeId=1
```

Here, we've commented out `storeid` so it doesn't cause an error. 

<p align="center">
  <img src="assets/css/img/Command injection/Comnd_inj5.png" alt="Command_injection" />
</p>

<p align="center">
  <img src="assets/css/img/Command injection/Comnd_inj6.png" alt="Command_injection" />
</p>

After processing the request, we see that the response returned the user 

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
