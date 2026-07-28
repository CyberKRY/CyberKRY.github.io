---
layout: post
title: "Lab: User ID controlled by request parameter with data leakage in redirect [Write-up]"
date: 2026-07-28
platform: portswigger
vulntype: access_control_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** User ID controlled by request parameter with data leakage in redirect

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab contains an access control vulnerability where sensitive information is leaked in the body of a redirect response.

To solve the lab, obtain the API key for the user `carlos` and submit it as the solution.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

After reading the problem description, we were given a hint that the vulnerability can be triggered when a user is redirected to another page. In short, we need to trigger a **`302`** status code, since it is responsible for redirecting the user to another page.

After trying a few different options, I managed to trigger a 302 error when I changed the parameter `/my-account?id=wiener` in the URL to `carlos.`

<p align="center">
  <img src="/assets/css/img/Access control/Access_control13.png" alt="access_control_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Access control/Access_control14.png" alt="access_control_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Access control/Access_control15.png" alt="access_control_vuln" />
</p>

If you look at these two requests, you can see that the cookies are different. By entering the name “carlos,” we were able to retrieve the cookie for the user “carlos.” If we send a GET request to our page, changing the cookie and ID to “carlos,” we can navigate to carlos's page and retrieve his API key.

<p align="center">
  <img src="/assets/css/img/Access control/Access_control16.png" alt="access_control_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Access control/Access_control17.png" alt="access_control_vuln" />
</p>

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
