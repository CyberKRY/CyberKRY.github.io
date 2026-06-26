---
layout: post
title: "PortSwigger — Reflected XSS into HTML context with nothing encoded [Write-up]"
date: 2025-11-10
platform: portswigger
vulntype: XSS
difficulty: Easy
category: write-up
tags: [xss, web]
---

## Overview

**Lab:** Reflected XSS into HTML context with nothing encoded  
**Platform:** PortSwigger  
**Difficulty:** Easy  

---

# Description

This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.

To solve the lab, perform a cross-site scripting attack that calls the `alert` function.

<h4>To accomplish this task, we can use this payload. </h4>

```
<script>alert(123)</script>
```
<p align="center">
  <img src="https://github.com/CyberKRY/CyberKRY.github.io/blob/main/assets/css/img/XXS/XXS1.png" alt="XXS" />
</p>

<b>After clicking Search, the script ran and brought up a window in the browser.</b>

<p align="center">
  <img src="https://github.com/CyberKRY/CyberKRY.github.io/blob/main/assets/css/img/XXS/XXS2.png" alt="XXS" width="700" />
</p>
