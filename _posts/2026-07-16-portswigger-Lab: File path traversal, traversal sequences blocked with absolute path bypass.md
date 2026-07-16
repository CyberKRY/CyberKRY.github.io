---
layout: post
title: "Lab: File path traversal, traversal sequences blocked with absolute path bypass [Write-up]"
date: 2026-07-16
platform: portswigger
vulntype: path_traversal
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** File path traversal, traversal sequences blocked with absolute path bypass

**Platform:** PortSwigger

**Difficulty:** Medium

---


## Description 

This lab contains a path traversal vulnerability in the display of product images.

The application blocks traversal sequences but treats the supplied filename as being relative to a default working directory.

To solve the lab, retrieve the contents of the `/etc/passwd` file.

## Solution

The goal of this exercise is to read the `/etc/passwd` file by exploiting a path traversal vulnerability.

The challenge description told us that the vulnerability can be triggered when displaying product images, so let's start by intercepting the request using Burp Suite.

We need to refresh the home page, and then we'll see our request for the product image.

The request contains a parameter that specifies which image should be loaded. Normally, this parameter contains the name of an image file, but since the application uses the provided value to locate the file on the server, we can try supplying our own path instead.

The lab description also mentions that the application blocks directory traversal sequences such as `../`. However, it still accepts absolute paths. An absolute path starts with `/`, which tells the operating system to begin searching from the root of the file system instead of the application's working directory.

Because of this, we can simply replace the image name with `/etc/passwd`. The application reads the requested file and returns its contents in the response, successfully solving the lab.

<p align="center">
  <img src="/assets/css/img/Path Traversal/PathT3.png" alt="path_traversal" />
</p>

<p align="center">
  <img src="/assets/css/img/Path Traversal/PathT6.png" alt="path_traversal" />
</p>
