---
layout: post
title: "Lab: Blind OS command injection with output redirection [Write-up]"
date: 2026-07-19
platform: portswigger
vulntype: command_injection
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Blind OS command injection with output redirection

**Platform:** PortSwigger

**Difficulty:** Medium

---


## Description 

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response. However, you can use output redirection to capture the output from the command. There is a writable folder at:

`/var/www/images/`

The application serves the images for the product catalog from this location. You can redirect the output from the injected command to a file in this folder, and then use the image loading URL to retrieve the contents of the file.

To solve the lab, execute the whoami command and retrieve the output.

## Solution

This lab is quite interesting. Our goal is to determine the current user by executing the whoami command. Since this is a blind OS command injection, the output of the command will not be returned in the HTTP response. However, the lab provides a writable directory located at /var/www/images/, where we can redirect the command output to a file. Because this directory is used to serve product images, we'll be able to retrieve the file through the application's image URL and read the result of the whoami command.

Let's start by testing the vulnerable parameter. I'll use the command `sleep 10`, and first I'll try to insert it into the `name` parameter. 

```python
|| sleep 10 ||

&name=test1 || sleep 10 || &email=test%40test.com&subject=test3&message=test4

URL:

&name=test1+||+sleep+10+||+&email=test%40test.com&subject=test3&message=test4

```

And that doesn't work, so let's move on to the “email” parameter

```python
|| sleep 10 ||

&name=test1&email=test%40test.com || sleep 10 || &subject=test3&message=test4

URL:

&name=test1&email=test%40test.com+||+sleep+10+||+&subject=test3&message=test4

```

<p align="center">
  <img src="/assets/css/img/Command injection/Comnd_inj10.png" alt="Command_injection" />
</p>

And yes, our request hung for 10 seconds, which means that the “email” parameter is vulnerable. 

Now our task is to redirect the output of the `whoami` command to a file so that we can read it later. 

In Linux, this is quite simple, and we create a payload like this 

```python
|| whoami > /var/www/images/whoami.txt ||
```

> It is important to specify this path, because this is the only location from which we can read the file

<p align="center">
  <img src="/assets/css/img/Command injection/Comnd_inj14.png" alt="Command_injection" />
</p>

We send the request, then go to the home page in our browser, right-click on any image, select `Open image in New Tab`, and then paste the path to our file into the URL bar. In the response, we’ll see our user, and we’ll have successfully completed the lab. 

<p align="center">
  <img src="/assets/css/img/Command injection/Comnd_inj11.png" alt="Command_injection" />
</p>

<p align="center">
  <img src="/assets/css/img/Command injection/Comnd_inj12.png" alt="Command_injection" />
</p>

<p align="center">
  <img src="/assets/css/img/Command injection/Comnd_inj13.png" alt="Command_injection" />
</p>

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
