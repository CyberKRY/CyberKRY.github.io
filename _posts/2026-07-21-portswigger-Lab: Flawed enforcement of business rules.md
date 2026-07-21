---
layout: post
title: "Lab: Flawed enforcement of business rules [Write-up]"
date: 2026-07-21
platform: portswigger
vulntype: business_logic_vuln
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Flawed enforcement of business rules

**Platform:** PortSwigger

**Difficulty:** Easy

---


## Description 

This lab has a logic flaw in its purchasing workflow. To solve the lab, exploit this flaw to buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: `wiener:peter`

## Solution

We're taken to the store's homepage, where we can see a few details. In this section, we see a discount promo code, but before we use it, let's log in to our wiener account.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln24.png" alt="Business_log_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln2.png" alt="Business_log_vuln" />
</p>

Our goal is to buy the Lightweight l33t leather jacket, so let's go to that item and add it to the cart. 

Since we don't have enough money to buy it, I decided to try all the methods from our previous lab assignments, but none of them worked, so I decided to enter the promo code and see what happens.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln25.png" alt="Business_log_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln26.png" alt="Business_log_vuln" />
</p>

We were given a $5 discount, but we still didn't have enough money, so I decided to try entering the promo code again, thinking that the app might accept the same promo code multiple times.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln27.png" alt="Business_log_vuln" />
</p>

and we weren't able to get the discount; the app is showing an error message again, stating that this promo code has already been entered 

After trying a few more methods without any success, I started looking for something else on the page; as I scrolled down, I saw this:

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln28.png" alt="Business_log_vuln" />
</p>

They're asking us to subscribe to their newsletter, so let's enter our email address 

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln29.png" alt="Business_log_vuln" />
</p>

Once we send it, we'll receive one more promo code 

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln30.png" alt="Business_log_vuln" />
</p>

Let's add it to our cart and see what discount we'll get 

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln31.png" alt="Business_log_vuln" />
</p>

It came out to $401—even with this discount, we don't have enough money. Let's try entering this promo code again.

We'll get a message saying that this coupon has already been used, so I tried entering the previous promo code again, and it worked. The app only checks the most recent promo code and doesn't check the previous ones. 

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln32.png" alt="Business_log_vuln" />
</p>

Let's try using these two promo codes one after the other until the price of the item fits our budget.

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln33.png" alt="Business_log_vuln" />
</p>

Right now, this product costs $0, so we can go ahead and buy it

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln34.png" alt="Business_log_vuln" />
</p>

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
