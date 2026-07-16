---
layout: post
title: "Lab: File path traversal, traversal sequences stripped with superfluous URL-decode [Write-up]"
date: 2026-07-16
platform: portswigger
vulntype: path_traversal
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** File path traversal, traversal sequences stripped with superfluous URL-decode

**Platform:** PortSwigger

**Difficulty:** Medium

---


## Description 

This lab contains a path traversal vulnerability in the display of product images.

The application blocks input containing path traversal sequences. It then performs a URL-decode of the input before using it.

To solve the lab, retrieve the contents of the  `/etc/passwd` file.

## Solution

The application blocks common path traversal sequences such as `../`, but it performs URL decoding after the validation step.

Because the input is checked before decoding, we can bypass the filter by using URL encoding. The application does not detect the traversal sequence during the validation stage, but after decoding it becomes a valid path traversal sequence and is processed by the server.

In this case, double URL encoding was required because the application decodes the input multiple times before using it.

After encoding the URL, our payload looks like this:

```python
..%252F..%252F..%252Fetc%252Fpasswd
```

After we send the request, we'll see a response containing the contents of the `/etc/passwd` file

<p align="center">
  <img src="/assets/css/img/Path Traversal/PathT6.png" alt="path_traversal" />
</p>

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
