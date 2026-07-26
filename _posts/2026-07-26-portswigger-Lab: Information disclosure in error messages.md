---
layout: post
title: "Lab: Information disclosure in error messages [Write-up]"
date: 2026-07-26
platform: portswigger
vulntype: information_disclosure
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Information disclosure in error messages

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab's verbose error messages reveal that it is using a vulnerable version of a third-party framework. To solve the lab, obtain and submit the version number of this framework.

## Solution

Let's start with the theory of what information disclosure is.

Information disclosure, also known as information leakage, is when a website unintentionally reveals sensitive information to its users. Depending on the context, websites may leak all kinds of information to a potential attacker, including:

Data about other users, such as usernames or financial information

Sensitive commercial or business data

Technical details about the website and its infrastructure

Now that we understand the theory, we can begin the practical assignment for this lab. In this lab, we will determine the version of the framework running on this website. 

When we go to the home page, we'll see various posts. Our goal is to find out the framework version. Let's try to trigger an error in the app; if the app is poorly implemented, we can extract useful information from this error. 

<p align="center">
  <img src="/assets/css/img/information disclosure/information disclosure1.png" alt="information_disclosure" />
</p>

I'll use Burp Suite to intercept the request when navigating to any page, and I'll try setting different values to trigger an error. 


In the `productId` field, you can substitute different values such as 0 or -1, leave it blank, or remove the parameter entirely, and monitor how the app reacts to these changes. In one attempt, I tried adding a single quote ', which successfully caused an error. 

<p align="center">
  <img src="/assets/css/img/information disclosure/information disclosure2.png" alt="information_disclosure" />
</p>

If we look at the error, we can see the information we need at the end: Apache Struts 2 2.3.31

<p align="center">
  <img src="/assets/css/img/information disclosure/information disclosure3.png" alt="information_disclosure" />
</p>

The answer will be: 2.3.31

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
