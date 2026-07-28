---
layout: post
title: "Lab: User ID controlled by request parameter [Write-up]"
date: 2026-07-28
platform: portswigger
vulntype: access_control_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** User role controlled by request parameter

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This challenge contains an IDOR vulnerability; we need to obtain the API key for the user `carlos.`

Let's log in to our account and take a look at the URL

```python
https://0a7d00070404b4168120572b00df0095.web-security-academy.net/my-account?id=wiener
```

If we change the user's ID to “carlos,” we'll be able to view carlos's private user page, which contains his API key. 

```python
https://0a7d00070404b4168120572b00df0095.web-security-academy.net/my-account?id=carlos
```

We send the API key in the response and complete the lab 

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
