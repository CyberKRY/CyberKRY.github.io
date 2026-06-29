---
layout: post
title: "SQL injection attack, querying the database type and version on Oracle [Write-up]"
date: 2026-06-29
platform: portswigger
vulntype: sqli
difficulty: Medium
category: write-up
tags: [sqli, web]
---

## Overview

**Lab:** SQL injection attack, querying the database type and version on Oracle

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

## Solution

## Step 1 – Identify the Injection Point

During the analysis of the website, it was discovered that the server uses the “category” parameter to construct an SQL query. Testing revealed that the value of this parameter can be modified in a way that affects the executed query, confirming the presence of an “SQL injection” vulnerability. 

## Step 2 – Determine the Number of Columns

Before using UNION SELECT, you need to determine how many columns the original query returns.

To do this, a sequential check is performed using ORDER BY.

When the value exceeds the number of existing columns, the application begins to return an error.

In this lab, we determine that the query contains two columns.

<p align="center">
  <img src="/assets/css/img/SQL/sql1.png" alt="SQLi" />
</p>

```python
Gifts'+ORDER+BY+1+--
Gifts'+ORDER+BY+2+--
Gifts'+ORDER+BY+3+--
```

## Step 3 – Check Compatible Data Types

The next task is to determine which columns can contain string values.

Since the lab uses Oracle Database, every SELECT statement must include a FROM clause. If the name of an existing table is unknown, you can use the built-in DUAL table, which is designed specifically for such cases.

<p align="center">
  <img src="/assets/css/img/SQL/sql2.png" alt="SQLi" />
</p>

<p align="center">
  <img src="/assets/css/img/SQL/sql3.png" alt="SQLi" />
</p>

```python
Gifts'+UNION+SELECT+'Hack','live'+FROM+dual+--
```

## Step 4 – Retrieve Database Version

Once the query structure has been defined, you can query the v$version view, which contains information about the Oracle Database version.

From this view, select the `banner` column, which contains the full version string.

Since the original query returns two columns, a NULL value is used in the second position to preserve the same result structure.

After executing the query, the application displays the database version string, marking the successful completion of the lab.

<p align="center">
  <img src="/assets/css/img/SQL/sql4.png" alt="SQLi" />
</p>

```python
Gifts'+UNION+SELECT+banner,+null+FROM+v$version+--
```
