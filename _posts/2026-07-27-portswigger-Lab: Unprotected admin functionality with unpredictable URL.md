---
layout: post
title: "Lab: Unprotected admin functionality with unpredictable URL [Write-up]"
date: 2026-07-27
platform: portswigger
vulntype: access_control_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Unprotected admin functionality with unpredictable URL

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab has an unprotected admin panel. It's located at an unpredictable location, but the location is disclosed somewhere in the application.

Solve the lab by accessing the admin panel, and using it to delete the user `carlos`.

## Solution

Our task is to find the admin panel and delete the user `carlos.` Let's do a manual crawl, explore the site, and try to find the information we need. 

I opened the source code, scrolled down a bit, and noticed this code snippet 

```python
var isAdmin = false;
if (isAdmin) {
   var topLinksTag = document.getElementsByClassName("top-links")[0];
   var adminPanelTag = document.createElement('a');
   adminPanelTag.setAttribute('href', '/admin-dvxz5s');
   adminPanelTag.innerText = 'Admin panel';
   topLinksTag.append(adminPanelTag);
   var pTag = document.createElement('p');
   pTag.innerText = '|';
  
```

```python
/admin-dvxz5s
```

The admin panel is accessible to regular visitors, so we can take advantage of this and delete the user `carlos.`

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
