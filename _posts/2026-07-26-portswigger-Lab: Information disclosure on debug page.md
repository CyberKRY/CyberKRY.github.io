---
layout: post
title: "Lab: Information disclosure on debug page [Write-up]"
date: 2026-07-26
platform: portswigger
vulntype: information_disclosure
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Information disclosure on debug page

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab contains a debug page that discloses sensitive information about the application. To solve the lab, obtain and submit the `SECRET_KEY` environment variable.

## Solution

In this problem, we need to discover the hidden path to the information we need—specifically, SECRET_KEY.

Let's start by doing some reconnaissance—let's just browse through everything on the site that might give us a clue. If we open the source code and scroll all the way to the bottom, we can see a comment that was most likely not removed by the developer. 

<p align="center">
  <img src="/assets/css/img/information disclosure/information disclosure4.png" alt="information_disclosure" />
</p>

Let's use this path to go to the page 

```python
/cgi-bin/phpinfo.php
```

We're taken to the phpinfo() page—PHP's built-in report on the current environment configuration. The task instructs us to find SECRET_KEY, so let's press CTRL + F and type in SECRET_KEY.

<p align="center">
  <img src="/assets/css/img/information disclosure/information disclosure5.png" alt="information_disclosure" />
</p>

And we find the key we need, which we can enter as the answer to complete the lab.

<p align="center">
  <img src="/assets/css/img/information disclosure/information disclosure6.png" alt="information_disclosure" />
</p>

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
