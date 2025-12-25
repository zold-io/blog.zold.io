---
layout: post
title: "Double Spending"
date: 2019-05-28
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Zold network has a simple counter-measure against
  double spending mistake and 51%-attack, here is
  how it works.
tags: architecture
---

The most important problem any decentralized digital payment system
has to solve is known as [double-spending](https://en.wikipedia.org/wiki/Double-spending).
Just like Blockchain, Zold may also become a victim of such an attack
or maybe even a technical mistake without evil intentions. What technically
may happen is that two different nodes present two different versions
of the same wallet, sending zolds in two different directions. How Zold
deals with this?

<!--more-->

Let's say a node has a wallet file with a single transaction, which
transfers 50 ZLD out. In some time, a new copy of this wallet file
is pushed to the node. In this new copy there is another transaction, which
sends the same 50 ZLD, but to a different address. What should a node do,
which version of the data it has to trust: the one it maintains or the one
arriving from another node?

The decision is made by a simple algorithm: If the transaction is
older than 24 hours, it will be overwritten; however, if the node,
which is suggesting to overwrite it, is a
[master]({% post_url 2018/12/2018-12-27-masters-and-edges %})
node, it will be overwritten.

This means that during the first 24 hours after the moment of transaction
creation it still can be overwritten by a simple
[weighed]({% post_url 2018/07/2018-07-22-nscore %}) majority of nodes.
The stronger group of Zold nodes will decide
where the payment should go. When this time period is over, all nodes
classify this transaction as a so called "legacy" and don't allow
anyone to overwrite it (can be turned off with `--skip-legacy` option).

There are a few possible threats to this algorithm.

First: what if the network stays split in two (or more) parts for more than
24 hours, and these parts are not connected? The probability of this
situation is extremely low, but this event may still happen. A group of
nodes may become disconnected from the network for some time and when
they get connected again, they will contain outdated information and will
refuse to listen to the majority. In order to prevent this from happening
[master]({% post_url 2018/12/2018-12-27-masters-and-edges %}) nodes
have a special status. They will always have enough power to overwrite
anything.

Second: what if master nodes get into conflict between each other? This may
happen too, if one master node gets one copy of the wallet and another one
has another copy. In order to prevent endless conflicts between them we
must always have an _odd_ number of master nodes. Thanks to that, they will
eventually make a mutual decision.

Third: what if someone sends a payment and then, within 24 hours,
overwrites it and sends it to a different destination, using a few nodes
with a _very big_ score? This may happen, and is known as 51%-attack. It is not
preventable and may only be resolved by the user who is accepting the payment.
If the amount of zolds you are expecting is large enough, make sure you
wait for 24 hours before making a shipment.

It's important to mention that this
24-hours interval is only temporarily that large. Technically, we can lower it
to just a few minutes, since an average confirmation time in Zold [network](http://www.zold.io/health.html)
at the moment is less than 140 seconds. The interval is not configurable
at the moment, but will be
[soon](https://github.com/zold-io/zold/issues/764),
via `--legacy-hours` command line option.
