---
layout: post
title: What Blockchain Is And What It Is Not
date: 2018-08-13
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Very often we misunderstand what Blockchain is and how
  much business value it really has; in many cases
  a simple MySQL database is more than sufficient.
tags: blockchain
---

Blockchain, obviously, is a hot [market trend](https://www.cnbc.com/2018/05/04/cryptocurrencies-and-blockchain-are-becoming-a-hot-trend-in-the-job-market.html).
Every single person I've met over the last few months has had a business
plan somewhere in the area of cryptocurrencies and, of course, Blockchain.
Very few of them, when I've been asking "Why Blockchain?" were capable of giving
any reasonable answer. The savviest ones have been saying that it's a very reliable and immutable
database. Others just claimed that "It's the future!"

<!--more-->

<iframe width="560" height="315" src="https://www.youtube.com/embed/48mtB40sKhs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Wikipedia at the time of writing [says](https://en.wikipedia.org/wiki/Blockchain)
that Blockchain is a "growing list of records"
and a "public ledger". I would actually say that it's a data maintenance
principle. It unites computers, which we don't trust,
into a group, which we can trust. Not entirely, but much more.

That's exactly the purpose of Blockchain and similar decentralized data
structures (like [Tangle](https://en.bitcoinwiki.org/wiki/IOTA)
or [Zold](https://www.zold.io)): to make a database trustworthy,
while each individual element of it is anonymous and that's why, by definition,
can't be trusted. Each server may corrupt the data and steal our money.
We would never trust our transactions to it. But we trust
them to a large group of such servers. Why?

Because all computers in the group vote for our transactions and the
majority decides which data wins. Then, we hope that the majority of servers
are legal and trustworthy, while the minority may be against us, trying
to steal our money. Since the amount of servers is big, the group is
trustworthy. We simply believe that the majority is always on our side.

Each server contains a full copy of the entire database, any time ready
to get into a fight with other servers and vote for the data, to achieve
so called _consensus_. This extreme redunancy of data is the price we pay
for the trust in Blockchain. For example, at the time of writing,
9,000+ Bitcoin servers maintain 173Gb of data _each_!

Thus, Blockchain is a suitable data storage solution, when
we _don't trust_ data maintainers. This is the only reasonable use case
for it.

On the other hand, it absolutely makes no sense to use Blockchain when
all group members trust each other. I can't really understand what
"private Blockchains" are for. If the network is private and owned by a single
organization, the servers are trustworthy. What is the point of duplicating
the data and paying for the data reduncancy. Why can't we just use MySQL?


