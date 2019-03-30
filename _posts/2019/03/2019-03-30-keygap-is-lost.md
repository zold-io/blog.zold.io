---
layout: post
title: Keygap is Lost? Sorry.
date: 2019-03-30
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  If your keygap is lost, you have no chance
  to recover access to your Zold wallet, sorry.
tags: algorithm
---

[Keygap]({% post_url 2018/07/2018-07-18-keygap %}),
as you know, is a small piece of text extracted from your private
RSA key in order to prevent your money loss in case of our database
is stolen. You get the Keygap when you register your account
with [WTS](https://wts.zold.io) or any other client of the WTS,
like a mobile wallet. What if you lose it? Can you restore the access
to your wallet somehow? Not really.

<!--more-->

Once again, when you create an account in [WTS](https://wts.zold.io),
our server creates a private RSA key for you and stores it in the
database. Then, it extracts a small part of it, removes it from the
RSA key text and _shows_ you. You should read it and save locally,
somewhere in your records. The screen should look like this:

<img src="/images/2019/03/wts-confirm.png"/>

This is your keygap. You have to save it and never share with anyone.
We don't keep it in our database. Nobody knows it, except you.

What do you do if you lost it?

You take a deep breath. Your zolds are lost forever.

Now it's time to click Restart and create a new wallet. You will
get a new keygap. Don't lose it this time.

Sorry, bro.
