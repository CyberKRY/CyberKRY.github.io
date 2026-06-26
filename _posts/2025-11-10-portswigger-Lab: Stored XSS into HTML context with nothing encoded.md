---
layout: post
title: "PortSwigger — Stored XSS into HTML context with nothing encoded [Write-up]"
date: 2025-11-10
platform: portswigger
vulntype: XSS
difficulty: Easy
category: write-up
tags: [xss, web]
---

## Overview

**Lab:** Stored XSS into HTML context with nothing encoded

**Platform:** PortSwigger  

**Difficulty:** Easy  

---

# Description

This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the `alert` function when the blog post is viewed.

## Solution
By going to the website, we can access any post. Scrolling down the page, we see a comment submission form, which potentially allows us to carry out our attack.

<p align="center">
  <img src="/assets/css/img/XXS/XXS3.png" alt="XXS" />
</p>

We can use this payload:

```
<script>alert(123)</script>
```
<p align="center">
  <img src="/assets/css/img/XXS/XXS4.png" alt="XXS" />
</p>

After posting, the comment will remain on the page, and anyone who visits this page will be vulnerable to attack.

<p align="center">
  <img src="/assets/css/img/XXS/XXS5.png" alt="XXS" />
</p>

