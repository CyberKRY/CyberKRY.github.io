---
layout: post
title: "Visible error-based SQL injection [Write-up]"
date: 2026-07-04
platform: portswigger
vulntype: sqli
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Visible error-based SQL injection

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab contains a SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie. The results of the SQL query are not returned.

The database contains a different table called users, with columns called username and password. To solve the lab, find a way to leak the password for the administrator user, then log in to their account.

## Error-based SQL Injection

Error-based SQL Injection is a technique that exploits database error messages to extract information. It works by intentionally triggering an SQL error (for example, using CAST() or other database-specific functions), causing the database to reveal information such as table names, column names, or even sensitive data in the error message.

# Solution

Let's start by intercepting a request using Burp Suite in the /login section 

If the request contains a TrackingId, you can forward it to the Repeater using the `CTRL + R` shortcut or by right-clicking on the request and selecting “Send to Repeater.”

The first step is to trigger a database error. If the application returns the error message to the user, it may reveal useful information about the database or even sensitive data.

Let's enter `'` to see how the database responds to our query

```python
TrackingId=x28E125nFWZTcsEN';
```

<p align="center">
  <img src="/assets/css/img/SQL/sql17.png" alt="SQLi" />
</p>

As you can see, the application constructs an SQL query using double quotes for the string. By adding our own quote, we “break” the query’s syntax, which results in a database error.

If we comment out everything after the quotation mark, the error will disappear. 

```python
TrackingId=x28E125nFWZTcsEN'--;
```

In our case, we will use the `CAST()` function, a standard SQL function used to convert data. In error-based SQL injection, we intentionally force CAST() to convert a non-numeric value (such as a password) into an integer. Since the conversion fails, the database generates an error message that may reveal the original value.

```python
' AND 1=CAST((SELECT 1) AS int)--
```

We begin with a simple query to verify that the CAST() function works in the current SQL context. Since 1 is already an integer, no conversion error is generated.

<p align="center">
  <img src="/assets/css/img/SQL/sql18.png" alt="SQLi" />
</p>

Next, we replace the integer with the result of a query that returns a string. Because the password cannot be converted into an integer, the database throws an error. If the application displays database errors, the value may be exposed in the error message.

```python
TrackingId=' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--;
```

```python
TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--;
```

<p align="center">
  <img src="/assets/css/img/SQL/sql19.png" alt="SQLi" />
</p>

<p align="center">
  <img src="/assets/css/img/SQL/sql20.png" alt="SQLi" />
</p>

In this query, we use LIMIT 1 because the server returns an error related to the maximum number of characters in the query; therefore, to ensure our query works correctly, we removed the cookie. 
