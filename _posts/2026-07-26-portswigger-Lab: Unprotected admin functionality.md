---
layout: post
title: "Lab: Unprotected admin functionality [Write-up]"
date: 2026-07-26
platform: portswigger
vulntype: access_control_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Unprotected admin functionality

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab has an unprotected admin panel.

Solve the lab by deleting the user carlos.

## Solution

Before we tackle this problem, let's review the theory of vulnerability. 

Access Control Vulnerabilities arise when an application does not properly verify whether a user is authorized to access a resource or perform an action. This can result in privilege escalation, allowing attackers to gain permissions beyond those originally assigned.

After manually crawling the entire site, I couldn't find anything useful, so I decided to use the common **`/robots.txt`** method to check which paths shouldn't be crawled. Since the robots.txt file was accessible, we were able to see the path to the admin panel.

```python
/administrator-panel
```

Let's delete the user “carlos” and finish the lab.

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
