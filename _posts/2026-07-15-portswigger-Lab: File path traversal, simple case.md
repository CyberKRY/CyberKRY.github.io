---
layout: post
title: "Lab: File path traversal, simple case [Write-up]"
date: 2026-07-15
platform: portswigger
vulntype: path_traversal
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** File path traversal, simple case

**Platform:** PortSwigger

**Difficulty:** Easy

---


# Description 

This lab contains a path traversal vulnerability in the display of product images.

To solve the lab, retrieve the contents of the `/etc/passwd` file.

assets/css/img/Path Traversal/PathT1.png

## Solution

The goal of this exercise is to read the /etc/passwd file by exploiting a path traversal vulnerability. 

The challenge description told us that the vulnerability can be triggered when displaying product images, so let's start by intercepting the request using Burp Suite

We need to refresh the home page, and then we'll see our query 

<p align="center">
  <img src="/assets/css/img/Path Traversal/PathT1.png" alt="path_traversal" />
</p>

 Let's click “Forward” 

<p align="center">
  <img src="/assets/css/img/Path Traversal/PathT2.png" alt="path_traversal" />
</p>

We can see that the images have started loading. Click on any query that contains the “filename” parameter and send it to the Repeater using the `CTRL + r` key combination. 

Let's enter our first payload 

<p align="center">
  <img src="/assets/css/img/Path Traversal/PathT3.png" alt="path_traversal" />
</p>

We'll take note of that response 

<p align="center">
  <img src="/assets/css/img/Path Traversal/PathT4.png" alt="path_traversal" />
</p>

With each attempt, we try to go higher until we reach the file 

And we manage to find a suitable payload that displays data from /etc/passwd in its output

```python
../../../etc/passwd
```

<p align="center">
  <img src="/assets/css/img/Path Traversal/PathT5.png" alt="path_traversal" />
</p>

<p align="center">
  <img src="/assets/css/img/Path Traversal/PathT6.png" alt="path_traversal" />
</p>



 
