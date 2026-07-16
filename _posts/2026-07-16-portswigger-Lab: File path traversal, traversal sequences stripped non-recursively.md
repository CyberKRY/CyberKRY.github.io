---
layout: post
title: "Lab: File path traversal, traversal sequences stripped non-recursively [Write-up]"
date: 2026-07-16
platform: portswigger
vulntype: path_traversal
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** File path traversal, traversal sequences stripped non-recursively

**Platform:** PortSwigger

**Difficulty:** Medium

---


## Description 

This lab contains a path traversal vulnerability in the display of product images.

The application strips path traversal sequences from the user-supplied filename before using it.

To solve the lab, retrieve the contents of the /etc/passwd file.

## Solution

In this problem, the app filters the path traversal sequences, but only those starting with ../—which we can use. 

Here's what our payload looks like 

```python
....//....//....//etc/passwd
```

Once our query has been filtered, it will look like this 

```python
../../../etc/passwd
```

which is a valid query and will return the file's contents 

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>

