---
layout: post
title: "PortSwigger — DOM XSS in innerHTML sink using source location.search [Write-up]"
date: 2025-11-10
platform: portswigger
vulntype: XSS
difficulty: Easy
category: write-up
tags: [xss, web]
---

## Overview

**Lab:** DOM XSS in innerHTML sink using source location.search

**Platform:** PortSwigger

**Difficulty:** Easy  

---

# Description 

This lab contains a DOM-based cross-site scripting vulnerability in the search blog functionality. It uses an `innerHTML` assignment, which changes the HTML contents of a `div` element, using data from `location.search`.

To solve this lab, perform a cross-site scripting attack that calls the `alert` function.

## Solution

When we enter the website, a search form appears where we can enter any data to track where it goes. 

<p align="center">
  <img src="/assets/css/img/XXS/XXS11.png" alt="XXS" />
</p>

Open Developer Tools

<p align="center">
  <img src="/assets/css/img/XXS/XXS12.png" alt="XXS" />
</p>

`innerHTML` interprets a string as HTML and creates DOM nodes from it. If `location.search` contains HTML/JS (like your `<img ... onerror=...>`), it will be turned into real elements on the page.

Using this, we can use this payload to carry out an attack.

```
<img src=1 onerror=alert(1)>
```
<p align="center">
  <img src="/assets/css/img/XXS/XXS13.png" alt="XXS" />
</p>

Why `onerror` fired specifically.
The browser tries to load the image from `src="1"`. The path `1` is not a valid URL, the load fails, and the `<img>` tag’s `onerror` handler runs. Since the `onerror` attribute contains the JavaScript `alert(1)`, it gets executed. In other words, your malicious code ended up in the DOM and executed in the page’s context.

<p align="center">
  <img src="/assets/css/img/XXS/XXS14.png" alt="XXS" />
</p>
