---
layout: post
title: "PortSwigger — Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped [Write-up]"
date: 2025-11-10
platform: portswigger
vulntype: XSS
difficulty: Medium
category: write-up
tags: [xss, web]
---

## Overview

**Lab:** Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description

This lab contains a reflected cross-site scripting vulnerability in the search blog functionality. The reflection occurs inside a template string with angle brackets, single, and double quotes HTML encoded, and backticks escaped. To solve this lab, perform a cross-site scripting attack that calls the alert function inside the template string.

# Solution

Let's enter our text and see where our input goes. 

<p align="center">
  <img src="/assets/css/img/XXS/XXS76.png" alt="XXS" />
</p>

After entering test, we can see that our text is placed in <script> in the variable var message. 

But there is an important detail: the quotation marks that enclose the input. 
Back quotation marks provide template strings where you can insert variable values using the ${name} syntax—it substitutes the variable content directly into the text.

So we can implement our alert through this function. 

```
${alert(1)}
```

<p align="center">
  <img src="/assets/css/img/XXS/XXS77.png" alt="XXS" />
</p>

And after entering, our attack worked successfully, and this is what the code looks like after the attack
