---
layout: post
title: Why We Were Losing Transactions?
date: 2018-07-17
author: yegor256
description: |
  Some wallets were losing transactions for a few recent
  weeks; here is a more or less detailed explanation
  of what was the problem.
tags: algorithm
---

Over the last few weeks we were experiencing a problem:
some transactions were disappearing from some wallets, quite frequently.
Such an annoying defect was caused by a bug in node-to-node connectivity.
Let me explain what exactly was happening.

<!--more-->

As our [White Paper](https://papers.zold.io/wp.pdf) explains, each wallet
is a plain text file, that contains a list of transaction. Transactions
can be positive, when money comes in, and negative, when the wallet sends
some money to another wallet. For example, this is my wallet (its
ID is `912ecc24b32dbe74` and it is in the middle):

{% highlight text %}
00000000000ff1ce    912ecc24b32dbe74    9c816e0285fd71dd
----------------    ----------------    ----------------
             ...
          -19.99 ->           +19.99
                               -4.95 ->           +24.95
                    ----------------    ----------------
                              +15.04              +24.95
{% endhighlight %}

Here, the wallet `00000000000ff1ce` sends me 19.99 ZLD and then I send
4.95 ZLD to the wallet `9c816e0285fd71dd`. Pretty simple, right?

Now, I open up a laptop and _pull_ my wallet from the network:

{% highlight bash %}
$ zold pull 912ecc24b32dbe74
{% endhighlight %}

Zold command line software connects to all available nodes and
attempts to _fetch_ the wallet from all of them. Then, since each node
may provide its own version of the wallet, the software has to decide
which information to trust. It basically trusts the majority.

...

