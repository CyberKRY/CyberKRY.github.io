---
layout: post
title: "Lab: Username enumeration via account lock [Write-up]"
date: 2026-07-12
platform: portswigger
vulntype: authentication
difficulty: Medium
category: write-up
tags: [web]
---

## Overview

**Lab:** Username enumeration via account lock

**Platform:** PortSwigger

**Difficulty:** Medium  

---

# Description 

This lab is vulnerable to username enumeration. It uses account locking, but this contains a logic flaw. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

Candidate [usernames](https://portswigger.net/web-security/authentication/auth-lab-usernames)

Candidate [passwords](https://portswigger.net/web-security/authentication/auth-lab-passwords)

## Solution

In this task, we need to find out a user’s name by triggering a login lockout. Simply put, we can only trigger a temporary login lockout for an account that actually exists in the system; therefore, our goal is to trigger this lockout by sending multiple requests using the same username. 

To complete this lab, we'll use Burp Suite and modify the username dictionary 

First, intercept the request and replace “username” with special characters 

Use this JavaScript code to create a dictionary that repeats each username 5 times. 

```javascript
const fs = require('fs');
const readline = require('readline');

const fileStream = fs.createReadStream('usernames.txt');
const rl = readline.createInterface({
   input: fileStream
})

let content = '';

async function processLines(){
   for await(const line of rl){
      for (i = 1; i < 6; i++){
         content += line + '\n';
      }
   }

   return content;
}

processLines().then((res) => {
   fs.writeFile('./5usernames.txt', res, (err) => {
      if (err) {console.error(err);}
   })
});
```

```javascript
node script.js
```

Insert this dictionary into the payloads

We launch the attack and notice that one of the responses is larger than the others. This happens because the username already exists in the system, and after too many login attempts, the server returns an additional message indicating that the account has been temporarily locked for one minute.

<p align="center">
  <img src="/assets/css/img/Authentication/Auth24.png" alt="Authentication" />
</p>

<p align="center">
  <img src="/assets/css/img/Authentication/Auth25.png" alt="Authentication" />
</p>

Now that we know the username, we can move on to the password.

Enter the username we found into the “username” field, copy the passwords provided by the lab, and then enter special characters into the “password” field, add the dictionary, and launch the attack. 

You may notice one query with the smallest size and no errors—that is the user's correct password. 

<p align="center">
  <img src="/assets/css/img/Authentication/Auth26.png" alt="Authentication" />
</p>

> ! It's better to do this method with a filter—it's more convenient—but for this lab, I managed without one.

Find out the password, go to the login form, log in using those credentials, and finish the lab.
