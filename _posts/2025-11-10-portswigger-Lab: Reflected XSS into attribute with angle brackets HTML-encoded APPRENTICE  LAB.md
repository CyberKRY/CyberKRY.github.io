---
layout: post
title: "PortSwigger — Reflected XSS into attribute with angle brackets HTML-encoded [Write-up]"
date: 2025-11-10
platform: portswigger
vulntype: XSS
difficulty: Easy
category: write-up
tags: [xss, web]
---

## Overview

**Lab:** Reflected XSS into attribute with angle brackets HTML-encoded
**Platform:** PortSwigger  
**Difficulty:** Easy  

---

# Description

This lab contains a reflected cross-site scripting vulnerability in the search blog functionality where angle brackets are HTML-encoded. To solve this lab, perform a cross-site scripting attack that injects an attribute and calls the alert function.

# Soulution

When we enter the laboratory, we will be taken to the search page, where we will first test where our input goes.

<p align="center">
  <img src="/assets/css/img/XXS/XXS19.png" alt="XXS" />
</p>

After entering the word test1, we can see that my input ended up in `<input value='test01'>`.

Therefore, we can use this payload for our attack. 

```
"onmouseover="alert(1)
```
<p align="center">
  <img src="/assets/css/img/XXS/XXS20.png" alt="XXS" />
</p>

This payload closes the value attribute and inserts onmouseover. 

Ultimately, the request will look like this

```
<input value="" onmouseover="alert(1)">
```

<b>`onmouseover` is an HTML attribute/event associated with mouse navigation. If an element has onmouseover="jsCode()", then when the cursor is hovered over that element, the browser will execute the specified JavaScript function.</b>

<p align="center">
  <img src="/assets/css/img/XXS/XXS21.png" alt="XXS" />
</p>
