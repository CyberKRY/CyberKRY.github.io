---
layout: post
title: "Lab: Insufficient workflow validation [Write-up]"
date: 2026-07-23
platform: portswigger
vulntype: business_logic_vuln
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Insufficient workflow validation

**Platform:** PortSwigger

**Difficulty:** Medium

---

## Description

This lab makes flawed assumptions about the sequence of events in the purchasing workflow. To solve the lab, exploit this flaw to buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

Just like in the previous labs, let's start by logging into the wiener account. Our goal is to buy a Lightweight l33t leather jacket. 

After going through the entire site and trying all the methods from the previous labs, I wasn't able to get anywhere, so I decided to try buying an item that fit my balance and see how the app handles it.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln44.png" alt="Business_log_vuln" />
</p>

After adding an item priced at $11.41 to my cart, I started intercepting the request in Burp Suite and clicked the “Order” button. 

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln45.png" alt="Business_log_vuln" />
</p>

I noticed that we send a POST request to the cart/checkout endpoint; after we send it, we receive a GET request with the parameter order-confirmed=true. We forward this request to Reapeter. 

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln46.png" alt="Business_log_vuln" />
</p>

Let's add our jacket to the cart and send that GET request via the repeater.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln48.png" alt="Business_log_vuln" />
</p>

and we were able to buy the product and pass the lab test

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
