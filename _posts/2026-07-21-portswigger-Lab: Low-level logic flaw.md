---
layout: post
title: "Lab: Low-level logic flaw [Write-up]"
date: 2026-07-21
platform: portswigger
vulntype: business_logic_vuln
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Low-level logic flaw

**Platform:** PortSwigger

**Difficulty:** Medium

---

## Description
This lab doesn't adequately validate user input. You can exploit a logic flaw in its purchasing workflow to buy items for an unintended price. To solve the lab, buy a "Lightweight l33t leather jacket".
You can log in to your own account using the following credentials: `wiener:peter`

## Solution
As in the previous labs, we start by logging into the wiener account and go looking for a weak spot in the purchasing flow. After trying all the same tricks I'd used in earlier labs, I got nowhere — this time the application seemed protected against those vectors.

So I decided to take a closer look at the quantity parameter in the cart. After intercepting the add-to-cart request in Burp Suite, I found the `quantity` parameter and tried setting it to 100.

The application returned an error saying you can't add more than 100 items at a time. I tried 99 instead, and that went through. This suggested the upper limit per request was somewhere around 99-100, but there was clearly no sanity check on the total cart value.

Since the only restriction was on the quantity per request, I decided to keep piling up the item count in the cart using Burp Intruder, sending requests like this over and over:

```python
productId=1&redir=PRODUCT&quantity=99
```

The idea was to push the cart total into an integer overflow, flipping it negative. In Intruder I used a Null payloads attack type with the required number of repeats, and in the Resource Pool I set it to Continue indefinitely with Max concurrent requests = 1, so the requests would fire strictly one after another — this way I wouldn't lose track of the exact moment the total went negative, since a mistake here would mean starting the whole process over.

I did a bit of math beforehand: to go from the overflow point down to zero, I'd need roughly 19854450 / 1337 ≈ 14850 units of the item, and since each request added 99 at a time, that's 14850 / 99 ≈ 150 requests. To be safe, I only sent 130 requests instead of 150, to avoid overshooting the target.
<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln35.png" alt="Business_log_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln36.png" alt="Business_log_vuln" />
</p>

After that, the cart total did indeed flip negative, and every additional item added from that point on brought it gradually closer to zero. I then switched to sending requests one at a time manually, carefully watching the running total, until it reached around -1250 dollars — one more step and the price would have exceeded my budget.

At that point I switched to the actual target item (Lightweight l33t leather jacket) and repeated the same process, adding it a little at a time until the final total dropped to 8 dollars — comfortably within the account's budget.
<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln37.png" alt="Business_log_vuln" />
</p>

<p align="center">
  <img src="/assets/css/img/Business logic vulnerabilities/Business_log_vuln38.png" alt="Business_log_vuln" />
</p>

From there I checked out without any issues — lab solved.
