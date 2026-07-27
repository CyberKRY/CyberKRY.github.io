---
layout: post
title: "Lab: User role controlled by request parameter [Write-up]"
date: 2026-07-27
platform: portswigger
vulntype: access_control_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** User role controlled by request parameter

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab has an admin panel at /admin, which identifies administrators using a forgeable cookie.

Solve the lab by accessing the admin panel and using it to delete the user `carlos`.

You can log in to your own account using the following credentials: `wiener:peter`

## Soltuion

We know how to get to the admin panel, and to access it, we need to change the cookie 

Let's intercept the request to `/admin`

<p align="center">
  <img src="/assets/css/img/Access control/Access_control1.png" alt="access_control_vuln" />
</p>

As we can see, there's an interesting setting here: `Admin=false`. Let's try intercepting requests and changing `Admin=true`. 

<p align="center">
  <img src="/assets/css/img/Access control/Access_control2.png" alt="access_control_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Access control/Access_control3.png" alt="access_control_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Access control/Access_control4.png" alt="access_control_vuln" />
</p>

We were able to delete the user `carlos` and complete the lab.

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
