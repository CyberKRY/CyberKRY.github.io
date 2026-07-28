---
layout: post
title: "Lab: User role can be modified in user profile [Write-up]"
date: 2026-07-27
platform: portswigger
vulntype: access_control_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** User role can be modified in user profile

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab has an admin panel at /admin. It's only accessible to logged-in users with a roleid of 2.

Solve the lab by accessing the admin panel and using it to delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

## Solution

After scanning the site using Burp Suite, I was unable to find the `roleid` parameter; as a result, I concluded that I needed to change my email address. 

I captured my request, and it looks like this:

<p align="center">
  <img src="/assets/css/img/Access control/Access_control5.png" alt="access_control_vuln" />
</p>

Let's send it to Repeater

<p align="center">
  <img src="/assets/css/img/Access control/Access_control6.png" alt="access_control_vuln" />
</p>

This is the response we received, and in it we can see the parameter we need: `roleid`. Right now, it's set to 1. Our task is to change it to 2 and gain admin access. 

If we modify our query as follows by adding this parameter—but this time with a value of 2—and send it

<p align="center">
  <img src="/assets/css/img/Access control/Access_control7.png" alt="access_control_vuln" />
</p>

```python
{
  "email":"test@test.com",
  "roleid": 2
}
```

<p align="center">
  <img src="/assets/css/img/Access control/Access_control8.png" alt="access_control_vuln" />
</p>

Then we'll see that an admin panel has appeared, which we can log into and use to delete the user “carlos,” thereby completing the lab.

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>


