---
layout: post
title: "Lab: User ID controlled by request parameter with password disclosure [Write-up]"
date: 2026-07-29
platform: portswigger
vulntype: access_control_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** User ID controlled by request parameter with password disclosure

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab has user account page that contains the current user's existing password, prefilled in a masked input.

To solve the lab, retrieve the administrator's password, then use it to delete the user carlos.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

Our task is to find out the administrator's password, log in to their account, and delete the user `carlos.`

Well, let's log in to our account 

<p align="center">
  <img src="/assets/css/img/Access control/Access_control18.png" alt="access_control_vuln" />
</p>

How can we see our password? We can't see it on the page. Let's intercept the password reset request.

<p align="center">
  <img src="/assets/css/img/Access control/Access_control19.png" alt="access_control_vuln" />
</p>

Now that we know we can retrieve the password and update it, our next step is to exploit the IDOR vulnerability to gain access to the administrator's profile.

Let's intercept the request to our page and change our name to `administrator`

<p align="center">
  <img src="/assets/css/img/Access control/Access_control20.png" alt="access_control_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Access control/Access_control21.png" alt="access_control_vuln" />
</p>

We've confirmed the vulnerability, and now we need to find out the password. Since we know how to do this, we intercept the password update request and can see the password in the response.

<p align="center">
  <img src="/assets/css/img/Access control/Access_control22.png" alt="access_control_vuln" />
</p>

Log in using this password 

<p align="center">
  <img src="/assets/css/img/Access control/Access_control23.png" alt="access_control_vuln" />
</p>

Now that we have access to the admin panel, let's go there and delete the user `carlos.`

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
