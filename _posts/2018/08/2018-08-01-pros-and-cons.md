---
layout: post
title: Pros and Cons
date: 2018-08-01
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Zold is an experimental non-blockchain cryptocurrency, which
  has its own pros and cons; here is a short summary of them.
tags: features
---

[Zold](https://www.zold.io) is an experimental digital currency, which
has its own pros and cons. It was designed as an alternative to existing
dominating cryptocurrencies, like [Bitcoin](https://bitcoin.org/en/),
[Etherium](https://www.ethereum.org/), and others. As
[explained earlier]({% post_url 2018/07/2018-07-08-mission %}),
its mission is to support the revolution of the software
development industry, [Zerocracy](https://www.zerocracy.com) is leading.
Explaining what Zold is, I often hear the question:
"What's so unique about it?" Here is the answer.

<!--more-->

Zold is not attempting to be the _best_ cryptocurrency. It has its own
features, which distinguishes it from other solutions. Bear in mind that
some of those solutions
may be more applicable than Zold to some business cases.

Here is the list:

  * There is **no general ledger**, each wallet has its own ledger;
    This makes Zold more scalable than Blockchain,
    where transactions have to follow each other sequentially.

  * Trust scores, the outcome of the **proof-of-work** algorithm, don't confirm data
    integrity statically, like [nonces](https://en.bitcoin.it/wiki/Nonce) in Blockchain, but are taken into account
    by the consensus protocol in runtime;
    This makes it easier to corrupt
    previous data, but helps maintain a larger amount of smaller servers.

  * The [double-spending problem](https://en.wikipedia.org/wiki/Double-spending)
    is resolved through a custom consensus protocol,
    which makes a decision according to a cumulative runtime trust score of all groups
    of nodes, which provide the data.

  * A wallet is a **plain text file**, which, unlike Bitcoin, doesn't require
    any software to keep it alive.

  * Unlike Blockchain, Zold is more resistant to [**51% attack**](https://bitcoin.org/en/glossary/51-percent-attack), since a full validation
    of any transaction down to the genesis root wallet could be done in a
    reasonable amount of time.

  * Unlike Blockchain, a node doesn't need to maintain the entire database
    (over [170Gb](https://www.statista.com/statistics/647523/worldwide-bitcoin-blockchain-size/) for Bitcoin),
    but may decide how many wallets to keep;
    This makes it possible to
    run micro nodes on cheap hardware, like mobile phones or even TVs and watches.

  * Transactions are not processed individually, but wallet modifications
    are delivered over the network in packages;
    This theoretically makes Zold [faster]({% post_url 2018/07/2018-07-30-how-fast-is-zold %})
    than any per-transaction network, including VISA and MasterCard.

A more detailed description of each feature can be found in our
[White Paper](https://papers.zold.io/wp.pdf).
