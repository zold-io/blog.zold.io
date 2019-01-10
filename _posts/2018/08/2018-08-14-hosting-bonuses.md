---
layout: post
title: Hosting Bonuses
date: 2018-08-14
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  While Zold network is still young and the amout of wallets
  and transactions is small, we support node maintainers
  with regular hosting bonuses.
tags: taxes
---

As you know from the [White Paper](https://papers.zold.io/wp.pdf), each
wallet has to pay _taxes_ to some network nodes in order to stay alive.
The taxes those nodes collect is the incentive for their owners to keep
servers online. In other cryptocurrencies this mechanism is called
_mining_, we call it taxes. However, while the network is still young,
the amount of taxes we can collect is way smaller than what server owners
pay for the hardware and electricity. That's why, to incentivize them,
we pay hosting bonuses, on top of taxes.

<!--more-->

At the moment, the mechanism is simple. Every hour [wts.zold.io](https://wts.zold.io) server
(you can see its [source code](https://github.com/zold-io/wts.zold.io))
selects the best 8 nodes out of those it can see and pays them 1 ZLD,
distributing equally. If it can't find eight nodes, it
uses as many as it can find.

Pay attention, there is no guarantee that your node will receive
the bonus every hour!

If it doesn't pay at all, feel free to [submit an issue](https://github.com/zold-io/wts.zold.io/issues)
to GitHub.

All payments are sent from the wallet
[17737fee5b825835](http://www.zold.io/ledger.html?wallet=17737fee5b825835)
(click the link to see the most recent payments sent).

If you want to stay on top of things and monitor our system closely,
just read this [Telegram channel](https://t.me/zold_wts).

<hr/>

We will update this article, if something changes.
