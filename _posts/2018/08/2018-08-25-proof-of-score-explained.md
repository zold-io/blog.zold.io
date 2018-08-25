---
layout: post
title: Proof of Score explained
date: 2018-08-25
author:
  name: Daniel Oltmanns
  github: oltdaniel
description: |
  Trust is one of the most critical parts
  in a decentralized system. This is why
  Zold implemented the Proof of Score in
  order to caclulate a trust score for
  nodes.
tags: algorithm
---

Trust between nodes in a decentralized system is an very important aspect of
security, since it ensures the overall robustness of the network. Zold
implemented this kind of trust in form of _Proof of Score_, which can tell
other participants in the network, wether a node is corrupted or not. To make
this component more understandable, this post will shortly point out the most
important functionalities.

<!--more-->

As described in the [Whitepaper](https://papers.zold.io/wp.pdf) the calculated
scores of each node is described as _Proof of Work_ and relies on the CPU power
provided by the node machine. It is based on timestamps and previous nonces,
which allows other network nodes to validate the provided score data. You may
wonder yourself now, how other participants can tell if a node is corrupted or
not by a _"simple"_ number and what this have to do with _Proof of Work_.

Actually, this _"simple"_ number can tell you more than you think. In order to
keep this number high, the node needs to invest a certain amount of computing
power, which needs to be at an consistent level, as scores will be erased after
24 hours. This already tells us, wether the node invests a lot of computing
power into the calculation and how long it has been offline before _(also
provided by 'hours_alive', which could be faked)_.

Something else that can be extracted from this _"simple"_ number is, wether the
nodes score is in the neighborhood of the network average. This can tell us, if
the node is actually providing enough computing power in order to be classified
as trusted.

----

What has this to do with _Proof of Score_?

In order make a decentralized network robust, a _Proof of Work_ isn't enough,
since it only tells us how much compuitng power a node invests. Zold however,
puts this work into score calculation, of which other participants can extract
information, that helps them to classify the nodes as corrupted or not. So on,
_Proof of Score_ is an new kind of proof for decentralized systems, that
includes much information about a nodes honesty.
