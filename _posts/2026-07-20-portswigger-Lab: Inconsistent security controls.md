---
layout: post
title: "Lab: Inconsistent security controls [Write-up]"
date: 2026-07-20
platform: portswigger
vulntype: business_logic_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Inconsistent security controls

**Platform:** PortSwigger

**Difficulty:** Easy

---


## Description 

This lab's flawed logic allows arbitrary users to access administrative functionality that should only be available to company employees. To solve the lab, access the admin panel and delete the user carlos.

## Solution

**Step 1**: Create an account

Go to the registration page, where you'll see a very important message: “If you work for DontWannaCry, please use your @dontwannacry.com email address.” In the task description, we were told that employees have administrative roles. 

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln18.png" alt="Business_log_vuln" />
</p>

**Step 2**: Log in to your account

But for now, let's create an account. At the top of the lab, there's an email client. Let's open it, copy our email address, and enter it in the registration field. A confirmation email will then be sent to our inbox; we'll click the link, and then log in to our account.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln19.png" alt="Business_log_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln20.png" alt="Business_log_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln21.png" alt="Business_log_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln22.png" alt="Business_log_vuln" />
</p>

**Step 3**: Change your email address

On our page, there is a “Change Email” field. Since the @dontwannacry.com domain is intended for employees, we can try to change the email address by entering any address, such as user@dontwannacry.com.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln17.png" alt="Business_log_vuln" />
</p>

**Step 4**: Delete the user “carlos”

After that, we can see that an admin panel has appeared. If we log in, we’ll see the users in the system, and among them is the one we need—Carlos. We need to delete his account and then complete the lab.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln23.png" alt="Business_log_vuln" />
</p>

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
