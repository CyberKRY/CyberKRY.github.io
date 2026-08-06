---
layout: post
title: "Lab: Basic SSRF against the local server [Write-up]"
date: 2026-08-06
platform: portswigger
vulntype: ssrf
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Basic SSRF against the local server

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at `http://localhost/admin` and delete the user `carlos`.

## Solution

`Server-Side Request Forgery (SSRF)` - is a web vulnerability that allows an attacker to force a server to send requests to arbitrary destinations. By abusing server-side request functionality, an attacker may access internal services, retrieve sensitive information, bypass network restrictions, or interact with systems that are not directly accessible from the internet.

Let's go to the home page and take a look around. 

On the home page, we can see posts and a login form. Let's click on a post. 

<p align="center">
  <img src="/assets/css/img/SSRF/ssrf1.png" alt="ssrf" />
</p>

At the bottom of each post, there is a button to check product availability 

Let's intercept the request using Burp Suite

<p align="center">
  <img src="/assets/css/img/SSRF/ssrf2.png" alt="ssrf" />
</p>

The parameter specifies a link to their API, which shows them the number of items. 

<p align="center">
  <img src="/assets/css/img/SSRF/ssrf3.png" alt="ssrf" />
</p>

Let's pass this request to Repeater and change the `stockApi` parameter to `localhost` with the path `/admin`.

```python
stockApi=http://localhost/admin/
```

After sending this request in the Render tab, we can see that we now have access to the admin panel. Our task is to delete the user “carlos,” so let's send another request to delete him. 

<p align="center">
  <img src="/assets/css/img/SSRF/ssrf4.png" alt="ssrf" />
</p>

```python
stockApi=http://localhost/admin/delete?username=carlos
```

<p align="center">
  <img src="/assets/css/img/SSRF/ssrf5.png" alt="ssrf" />
</p>

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
