---
layout: post
title: "Lab: Authentication bypass via OAuth implicit flow [Write-up]"
date: 2026-09-10
platform: portswigger
vulntype: oauth
difficulty: Easy
category: write-up
tags: [web]
---

## Overview

**Lab:** Authentication bypass via OAuth implicit flow

**Platform:** PortSwigger

**Difficulty:** Easy

---

## Description

This lab uses an OAuth service to allow users to log in with their social media account. Flawed validation by the client application makes it possible for an attacker to log in to other users' accounts without knowing their password.

To solve the lab, log in to Carlos's account. His email address is `carlos@carlos-montoya.net`.

You can log in with your own social media account using the following credentials: `wiener:peter`.

## Solution

Let's start with the basics: what is OAuth?

OAuth is a commonly used authorization framework that enables websites and web applications to request limited access to a user's account on another application. Crucially, OAuth allows the user to grant this access without exposing their login credentials to the requesting application. This means users can fine-tune which data they want to share rather than having to hand over full control of their account to a third party.

The basic OAuth process is widely used to integrate third-party functionality that requires access to certain data from a user's account. For example, an application might use OAuth to request access to your email contacts list so that it can suggest people to connect with. However, the same mechanism is also used to provide third-party authentication services, allowing users to log in with an account that they have with a different website.

Now that we know what OAuth is, we can get started on the lab. 

## Step 1

Let's go to the login page. With the correct credentials, we'll log in using the “wiener” user account, but first, let's open Burp Suite and intercept every request we make. 

<p align="center">
  <img src="/assets/css/img/OAuth/oauth7.png" alt="oauth" />
</p>

## Step 2 

We've logged in. Let's take a look at our requests in the Burp Suite history. We'll see that the authentication process involved several steps.

First, a request is sent to the OAuth server with parameters such as `client_id`, `redirect_uri`, `response_type`, `nonce`, and `scope`. These parameters specify which application is requesting authorization, where to redirect the user, and what data the application is requesting.

After successful authentication, the server returns an access token, which the application then uses to retrieve information about the user.

In the next request, we can see this token, and in the response—JSON containing user data, such as username, email, and other parameters.


## Step 3

We are primarily interested in the `/authenticate` request in JSON format, in which we specify an email address, a username, and a token.

<p align="center">
  <img src="/assets/css/img/OAuth/oauth1.png" alt="oauth" />
</p>

After sending the request, we'll see that there are no errors and that we received a 302 status code and a cookie. So, our task is to log in to Carlos's account; we know his email address, so let's try changing the “username” and “email” parameters. 

<p align="center">
  <img src="/assets/css/img/OAuth/oauth2.png" alt="oauth" />
</p>

<p align="center">
  <img src="/assets/css/img/OAuth/oauth3.png" alt="oauth" />
</p>

We send a request and receive a cookie for the user “carlos” 

<p align="center">
  <img src="/assets/css/img/OAuth/oauth4.png" alt="oauth" />
</p>

Now we can use them to log in to carlos's account. There are several ways to do this, but I used Burp Suite: I right-clicked on our request, selected “Request in browser in original session,” copied the link, and pasted the URL into the address bar. 

> ! It's important to enable the listener in your browser; it won't work without it.

<p align="center">
  <img src="/assets/css/img/OAuth/oauth5.png" alt="oauth" />
</p>

<p align="center">
  <img src="/assets/css/img/OAuth/oauth6.png" alt="oauth" />
</p>

And we successfully gained access to the “carlos” account 

---
<div align="center">

This post is licensed under  
<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>.

© 2026 CyberKRY

</div>
