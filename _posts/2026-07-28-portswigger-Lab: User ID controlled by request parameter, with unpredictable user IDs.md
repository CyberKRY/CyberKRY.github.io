---
layout: post
title: "Lab: User ID controlled by request parameter, with unpredictable user IDs [Write-up]"
date: 2026-07-28
platform: portswigger
vulntype: access_control_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** User ID controlled by request parameter, with unpredictable user IDs

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab has a horizontal privilege escalation vulnerability on the user account page, but identifies users with GUIDs.

To solve the lab, find the GUID for carlos, then submit his API key as the solution.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

Just like in the previous lab, we need to retrieve the API key for the user “carlos,” but now each user has a unique ID, so we can't just change the username. 

After trying various methods, I came across the posts on the page and noticed that a user named carlos had posted once; you can see that his name is a hyperlink that takes you to another page. 

<p align="center">
  <img src="assets/css/img/Access control/Access_control9.png" alt="access_control_vuln" />
</p>

<p align="center">
  <img src="assets/css/img/Access control/Access_control10.png" alt="access_control_vuln" />
</p>

If you intercept the request using Burp Suite, you'll see that the ID is different from ours. Let's copy it and go to our profile page.

<p align="center">
  <img src="assets/css/img/Access control/Access_control11.png" alt="access_control_vuln" />
</p>

<p align="center">
  <img src="assets/css/img/Access control/Access_control12.png" alt="access_control_vuln" />
</p>

And yes, there's also an IDOR vulnerability here that allowed us to view the user's profile; we copy the API key, send it back in the response, and complete the lab.

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
