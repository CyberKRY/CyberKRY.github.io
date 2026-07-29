---
layout: post
title: "Lab: URL-based access control can be circumvented [Write-up]"
date: 2026-07-29
platform: portswigger
vulntype: access_control_vuln
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** URL-based access control can be circumvented

**Platform:** PortSwigger

**Difficulty:** Medium

---

## Description

In the problem description, we were told that the service uses a framework that supports the `X-Original-URL` header.

`X-Original-URL` is a non-standard HTTP header used by some reverse proxies and web servers to pass the original request URL to the backend application. If the application trusts this header for request routing, a misconfiguration may allow an authorization bypass.

And for the routing example:

```python
/profile
   ↓
/internal/profile
```

The client requests the public path `/profile`, which is internally rewritten by the reverse proxy to `/internal/profile`. The backend processes the internal route, while the original path can be preserved in the `X-Original-URL` header.

When we visit the site, we can see that there is an “admin panel” page, but we are denied access. Let's use Burp Suite to intercept the request. 

<p align="center">
  <img src="/assets/css/img/Access control/Access_control27.png" alt="access_control_vuln" />
</p>

Let's use the `X-Original-URL` header and, based on the theory, try to access the admin panel.

<p align="center">
  <img src="/assets/css/img/Access control/Access_control24.png" alt="access_control_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Access control/Access_control25.png" alt="access_control_vuln" />
</p>

We were able to log into the admin panel, but if we try to delete a user, it still won't work. 

Therefore, continuing to apply this theory, we can send a request of this type 

<p align="center">
  <img src="/assets/css/img/Access control/Access_control26.png" alt="access_control_vuln" />
</p>

```python
GET ?username=carlos HTTP/2

X-Original-Url: /admin/delete
```

We were able to delete the user `carlos` and complete the lab.

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
