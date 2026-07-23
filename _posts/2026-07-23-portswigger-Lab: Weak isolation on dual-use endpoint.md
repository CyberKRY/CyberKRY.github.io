---
layout: post
title: "Lab: Weak isolation on dual-use endpoint [Write-up]"
date: 2026-07-23
platform: portswigger
vulntype: business_logic_vuln
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Weak isolation on dual-use endpoint

**Platform:** PortSwigger

**Difficulty:** Medium

---

## Description

This lab makes a flawed assumption about the user's privilege level based on their input. As a result, you can exploit the logic of its account management features to gain access to arbitrary users' accounts. To solve the lab, access the administrator account and delete the user carlos.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

The first thing we'll do is log in to our wiener account.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln2.png" alt="Business_log_vuln" />
</p>

We're taken to a page where we can change a user's password. The task description told us that we need to gain access to the administrator account, so let's try changing the password for that account.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln39.png" alt="Business_log_vuln" />
</p>

Let's also intercept the request using Burp Suite 

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln40.png" alt="Business_log_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln41.png" alt="Business_log_vuln" />
</p>

We were unable to do this because we do not have a valid user password. 

What if we tried removing the “current-password” parameter entirely?

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln42.png" alt="Business_log_vuln" />
</p>

```python
&username=administrator&new-password-1=peter&new-password-2=peter
```

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln43.png" alt="Business_log_vuln" />
</p>

And we were able to change the password for “administrator.” Let's log in to the account using the new password. 

After logging in to the administrator account, the administrator panel will appear at the top of the screen, where you can delete users. Our target is Carlos, so delete him and finish the hands-on exercise.

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
