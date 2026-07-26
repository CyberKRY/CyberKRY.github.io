---
layout: post
title: "Lab: Source code disclosure via backup files [Write-up]"
date: 2026-07-26
platform: portswigger
vulntype: information_disclosure
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Source code disclosure via backup files

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab leaks its source code via backup files in a hidden directory. To solve the lab, identify and submit the database password, which is hard-coded in the leaked source code.

## Solution

Just like in the previous labs, we'll start by exploring the tree and see if we can find anything interesting 

I looked all over the site but couldn't find anything useful, so I started searching for hidden directories 

One of the most popular is `/robots.txt`

> It could also have been discovered using tools such as gobuster, dirsearch, and others

`/robots.txt` is a text file on a website that tells search engine bots (such as Googlebot) which pages or sections they can or cannot crawl.

When we go to this page, we can see the following content

```python
User-agent: *

Disallow: /backup
```

If we navigate to the /backup directory, we'll see the `ProductTemplate.java.bak` file, which we can download and view

```python

        ConnectionBuilder connectionBuilder = ConnectionBuilder.from(
                "org.postgresql.Driver",
                "postgresql",
                "localhost",
                5432,
                "postgres",
                "postgres",
                "n75dotw68qa0tcyh43seg3jyrs1gijew"
        ).withAutoCommit();
        try
        {
```

```python
n75dotw68qa0tcyh43seg3jyrs1gijew
```
According to the task description, we need the encrypted password—it's clearly visible here—so let's copy it and send it. 

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
