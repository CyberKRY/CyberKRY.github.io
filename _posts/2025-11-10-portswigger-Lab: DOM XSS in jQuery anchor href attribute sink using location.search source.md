---
layout: post
title: "PortSwigger — DOM XSS in jQuery anchor href attribute sink using location.search source [Write-up]"
date: 2025-11-10
platform: portswigger
vulntype: XSS
difficulty: Easy
category: write-up
tags: [xss, web]
---

## Overview

**Lab:** DOM XSS in jQuery anchor href attribute sink using location.search source

**Platform:** PortSwigger

**Difficulty:** Easy  

---

# Description

This lab contains a DOM-based cross-site scripting vulnerability in the submit feedback page. It uses the jQuery library's `$` selector function to find an anchor element, and changes its `href` attribute using data from `location.search`.

To solve this lab, make the "back" link alert `document.cookie`.

# Solution

When we go to the website, we can see the Submit feedback button. We click on it and enter the page. 

We have this URL address:

```
https://0a4e00b003caf7268008f34c009300fc.web-security-academy.net/feedback?returnPath=/feedback
```
<p align="center">
  <img src="/assets/css/img/XXS/XXS15.png" alt="XXS" />
</p>

We need to check where our data goes. Let's replace /feedback with our symbols. 

```
https://0a4e00b003caf7268008f34c009300fc.web-security-academy.net/feedback?returnPath=abc
```
Then we enter the developer tools.

Using the search engine, we search for our input (CTRL + F)

<p align="center">
  <img src="/assets/css/img/XXS/XXS16.png" alt="XXS" />
</p>

Since our parameter is included in the href, we can use this payload.

```
https://0a4e00b003caf7268008f34c009300fc.web-security-academy.net/feedback?returnPath=javascript:alert(123)
```
After that, if we click <Back, DOM XXS will work.

<p align="center">
  <img src="/assets/css/img/XXS/XXS17.png" alt="XXS" />
</p>
