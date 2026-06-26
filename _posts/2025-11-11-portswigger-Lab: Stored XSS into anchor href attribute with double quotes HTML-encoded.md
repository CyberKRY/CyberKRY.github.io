---
layout: post
title: "PortSwigger — Stored XSS into anchor href attribute with double quotes HTML-encoded [Write-up]"
date: 2025-11-11
platform: portswigger
vulntype: XSS
difficulty: Easy
category: write-up
tags: [xss, web]
---

## Overview

**Lab:** Stored XSS into anchor href attribute with double quotes HTML-encoded
**Platform:** PortSwigger  
**Difficulty:** Easy  

---

# Description

This lab contains a stored cross-site scripting vulnerability in the comment functionality. To solve this lab, submit a comment that calls the alert function when the comment author name is clicked.

# Solution

When entering any message, we can write a comment. Let's send the data to see where it is entered. 

<p align="center">
  <img src="/assets/css/img/XXS/XXS22.png" alt="XXS" />
</p>

By entering the developer tools and finding our input, we can see that the website we specified is included in the href parameter. Now that the comment has been sent, we can access this website by clicking on the user's nickname.

<p align="center">
  <img src="/assets/css/img/XXS/XXS23.png" alt="XXS" />
</p>

And this prompts us to attack this point by changing the site to our payload. 

<p align="center">
  <img src="/assets/css/img/XXS/XXS24.png" alt="XXS" />
</p>

Now, if we click on the user's nickname, XXS will work.

<p align="center">
  <img src="/assets/css/img/XXS/XXS25.png" alt="XXS" />
</p>
