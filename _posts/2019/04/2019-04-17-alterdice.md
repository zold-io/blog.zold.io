---
layout: post
title: "AlterDice.com: The First Exchange Trading ZLD Coins"
date: 2019-04-17
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Zold was listed at AlterDice yesterday and is available
  for trading against USD, BTC, and ETH; this is great
  news for our team!
tags: architecture
---

<img src="/images/2019/04/alterdice-logo.png" style="float:right;width:128px;margin-left:1em;"/>

We're glad to share great news. Zold, an alternative non-Blockchain
cryptocurrency, which we started to develop in February 2018, was
listed by the first crypto exchange: [AlterDice](https://alterdice.com).
ZLD is traded against BTC, USD, and ETH and its price is 0.0002704 BTC
at the moment. Let me tell you a short story of how we managed
to get there and what it took to connect Zold to their software platform.

<!--more-->

First of all, I have to tell you that the situation with crypto exchanges
on the market is a tragedy now. We tried to discuss our product with
almost 40 of them and half of them were an obvious scam. They were
either asking us for some money upfront with no guarantee, or were not
understanding what non-Blockchain meant at all. It seems that the crypto
market is so young now that the majority of players are far from being
serious. Hopefully, this situation will change in some time.

Second, our protocol of data management is very different from what
Blockchain-based cryptocurrencies provide, like Bitcoin or Ethereum. We don't
have a block explorer, we have no transaction hashes, payment
addresses look different in Zold, and even transaction processing
algorithm works differently, comparing to Bitcoin, for example.
All these technical differences scare the majority of traditional exchanges.

Having this all in mind, it is easy to understand how much we appreciate
the work AlterDice did for us. The integration took almost three weeks
and we managed to resolve a lot of technical issues on our side, thanks
to their feedback and suggestions. We integrated them with
our [WTS](https://wts.zold.io) platform, via RESTful HTTP API.

You can try it yourself. Just go to [their website](https://alterdice.com),
create an account, fund it with ZLD or BTC, and
place an order to either sell or buy ZLD.

Thanks, folks from AlterDice!
