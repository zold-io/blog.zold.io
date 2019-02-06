---
layout: page
title: Roadmap
date: 2018-07-08
author: yegor256
permalink: roadmap.html
description: |
  Zold is a community driven software project, which
  has an ambitious and a very challenging roadmap;
  you are welcome to join.
tags: strategy
---

[Zold](https://www.zold.io) is an experimental community-driven project.
It has a very ambitious technical roadmap.
[Join us](https://t.me/zold_io) in Telegram to make this plan real.

<!--more-->

This is our current technical focus (most urgent and critical are at the top):

  * Zold-Stress doesn't demonstrate high-speed results; we have to investigate
    what's going on and what need to be fixed.

  * Large Traffic: we send wallet content too frequently between nodes, while
    it's possible to optimize that and return "Not-Modified" for all FETCH
    operations, if they request the content they already have. We may also
    want to delivery multiple wallets in one HTTP request ("packaged delivery")
    and delivery partial content.

  * Database: currently Zold node software keeps wallet data in files, which
    makes it difficult to manage and slow. Would be great to introduce
    a database-backed persistence layer, with SQLite, for example.

History:

6-Feb-19:<br/>
Queue Overflow problem is gone.
The amount of wallets "in process" was growing in some
nodes sometimes; this didn't sound like a valid behavior and most
likely meant some dead loops. Indeed, there were duplicate
PUSH operations accomplished by most nodes. The problem has been
fixed.

20-Jan-19:<br/>
Memory leakage has been [fixed](https://github.com/zold-io/zold/issues/672).
Thanks to that the entire network is stable now. Server software runs
without interruption for days. This was a bug (or a feature) in Ruby,
but we weren't able to find it for a few months.

25-Dec-18:<br/>
`--tolerate-edges` [introduced](https://github.com/zold-io/zold/issues/633) to increase security;
`--tolerate-quorum` [introduced](https://github.com/zold-io/zold/issues/639) to increase security;
[legacy](https://github.com/zold-io/zold/issues/643) wallet content to prohibit overwrites in 24 hours;
`--shallow` merge [introduced](https://github.com/zold-io/zold/issues/642);
"Hungry" wallets [added](https://github.com/zold-io/zold/issues/640), to improve wallets distribution mechanism;
[On-demand](https://github.com/zold-io/zold/issues/650) wallet content delivery to optimize traffic;
Garbage collection [introduced](https://github.com/zold-io/zold/issues/622) to remove empty and old wallets;
`repo` JSON attribute [added](https://github.com/zold-io/zold/issues/620) to see the difference between Zold implementations;
Score strength [increased](https://github.com/zold-io/zold/issues/619) to 8.

15-Nov-18:<br/>
[Zold-stress](https://github.com/zold-io/zold-stress), a stress-test automated
command like toolkit released.
Score calculating code moved to its own repository [zold-score](https://github.com/zold-io/zold-score),
and C/C++ implementation introduced.
Node aliases introduced in [#249](https://github.com/zold-io/zold/issues/249).
HTTP requests performance improved in [#176](https://github.com/zold-io/zold/issues/176).
Reboot mechanism is connected to the RubyGems website, in [#181](https://github.com/zold-io/zold/issues/181).

30-Jul-18:<br/>
[#412](https://github.com/zold-io/zold/issues/412):
Scores are being calculated in a separate process, which
makes HTTP front-end a few times faster (but still pretty slow).

22-Jul-18:<br/>
[#399](https://github.com/zold-io/zold/issues/399):
PUSH, PULL, and UPDATE are multi-threaded now, which makes
them much faster than before.

14-Jul-18:<br/>
[#402](https://github.com/zold-io/zold/issues/402):
Critical bugs with nodes connectivity were fixed,
the network is stable (over 70 nodes).

8-Jul-18:<br/>
This [blog](https://github.com/zold-io/blog.zold.io)
has been started and the first article has been published.

2-Jul-18:<br/>
The first version of the
[Green Paper](https://papers.zold.io/green-paper.pdf) has been published.

16-Jun-18:<br/>
We started to recruit Ruby developers on
[StackOverflow](https://stackoverflow.com/jobs/194602/brave-ruby-developer-for-a-new-cryptocurrency-zerocracy),
to build a team.

28-May-18:<br/>
The first transaction has been sent.

20-May-18<br/>
The first version of the [White Paper](https://papers.zold.io/wp.pdf) has been published.

12-May-18:<br/>
Version [0.1](https://github.com/zold-io/zold/tree/0.1) has been
[released](https://rubygems.org/gems/zold/versions/0.1) to RubyGems.

29-Jan-18:<br/>
The idea was born.
It became obvious that having our own cryptocurrency would be
benefitial for [Zerocracy](https://www.zerocracy.com).


