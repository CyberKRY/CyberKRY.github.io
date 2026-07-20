---
layout: post
title: "Lab: Excessive trust in client-side controls [Write-up]"
date: 2026-07-20
platform: portswigger
vulntype: business_logic_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Excessive trust in client-side controls

**Platform:** PortSwigger

**Difficulty:** Easy

---


## Description 

This lab doesn't adequately validate user input. You can exploit a logic flaw in its purchasing workflow to buy items for an unintended price. To solve the lab, buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: wiener:peter

## Solution

Let's start with the theory. What are business logic vulnerabilities?

**Business logic vulnerabilities are flaws in the design and implementation of an application that allow an attacker to elicit unintended behavior. This potentially enables attackers to manipulate legitimate functionality to achieve a malicious goal. These flaws are generally the result of failing to anticipate unusual application states that may occur and, consequently, failing to handle them safely.**

According to the problem description, we need to buy a jacket by exploiting a logical error in the purchasing process. 

When we go to the home page, we can see various products next to “My Account” and our shopping cart. Let's log in to our wienier account.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln2.png" alt="Command_injection" />
</p>


We need to buy a Lightweight l33t leather jacket, so let's choose this item.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln1.png" alt="Command_injection" />
</p>

If we scroll down, we'll see a field for selecting the quantity of items and an “Add to Cart” button. Let's select 1 item and click “Add to Cart.”

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln3.png" alt="Command_injection" />
</p>

Next, let's go to our shopping cart, where we can see our items and, most importantly, the balance in our account. If we try to purchase an item by clicking “Place Order,” we obviously won't be able to because we don't have enough money. 

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln4.png" alt="Command_injection" />
</p>

On this page, we can add more items, remove items, redeem a coupon, and purchase items. I tried to intercept the request here but couldn't get it to work, so let's go back to the page where we were just adding items to the cart.

Let's try to intercept the request here using Burp Suite

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln5.png" alt="Command_injection" />
</p>

And as we can see, we have the following parameters:

```python
productId=1&redir=PRODUCT&quantity=1&price=133700
```

We're interested in the “price” parameter, so let's adjust the price of this item so that it fits within our budget. 

```python
productId=1&redir=PRODUCT&quantity=1&price=1
```

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln6.png" alt="Command_injection" />
</p>

Let's submit the request and go to our shopping cart

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln7.png" alt="Command_injection" />
</p>

And yes, the item's price is now $0.01. Now click “Buy Item” and complete the lab.

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
