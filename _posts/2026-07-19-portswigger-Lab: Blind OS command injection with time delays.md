---
layout: post
title: "Lab: Blind OS command injection with time delays [Write-up]"
date: 2026-07-19
platform: portswigger
vulntype: command_injection
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Blind OS command injection with time delays

**Platform:** PortSwigger

**Difficulty:** Medium

---


## Description 

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response.

To solve the lab, exploit the blind OS command injection vulnerability to cause a 10 second delay.

## Solution

In this exercise, we need to identify a blind OS command injection vulnerability by triggering a time delay in the server's response. Since the application does not return the output of executed commands, we'll use a delay-based payload to confirm whether our command is being executed.

The first thing we'll do is identify the vulnerable input. According to the lab description, the vulnerability is located in the email parameter of the feedback form, so we'll intercept the request and test it by injecting a command that causes the server to pause before responding.

There are several commands that can cause a request to hang for a few seconds, but I'll use `sleep 10`. 

From our cheat sheets, we know that we can include a query using these types of quotes 

```python
&
&&
|
||
```

**Step 1**: Find the form for submitting feedback, fill in the information, and use Burp Suite to intercept the request 

<p align="center">
  <img src="/assets/css/img/Command injection/Comnd_inj7.png" alt="Command_injection" />
</p>

<p align="center">
  <img src="/assets/css/img/Command injection/Comnd_inj8.png" alt="Command_injection" />
</p>

After trying to enter a payload in the “name” field, I was unable to execute the command, so we'll continue testing and move on to the “email” parameter. 

**Step 2**: Insert our payload into the request 

There are quite a few possible approaches to this lab assignment, but the simplest one looks like this: 

```python
|| sleep 10 ||
```

Let's paste it and send it 

```python
&name=test1&email=test@test.com || sleep 10 ||&subject=test3&message=test4

URL:

&name=test1&email=test@test.com+||+sleep+10+||&subject=test3&message=test4
```

<p align="center">
  <img src="/assets/css/img/Command injection/Comnd_inj9.png" alt="Command_injection" />
</p>

After sending the request, we can see that the server freezes for 10 seconds, which confirms the vulnerability. 

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
