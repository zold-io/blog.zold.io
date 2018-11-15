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

  * Stabilize zold-stress for big numbers, and reach 100 tps
  * Get rid of memory leakage
  * Packaged PUSH and FETCH
  * Incremental PUSH and FETCH (to avoid full wallet data transfer)
  * [#554](https://github.com/zold-io/zold/issues/554): Zombie processes
  * [#315](https://github.com/zold-io/zold/issues/315): Make UPDATE more efficient, via `mtime()`
  * [#279](https://github.com/zold-io/zold/issues/279): Wallet aliases
  * [#235](https://github.com/zold-io/zold/issues/235): More effective wallet spreading mechanism
  * [#403](https://github.com/zold-io/zold/issues/403): Make node errors visible via HTTP
  * [#230](https://github.com/zold-io/zold/issues/230): `--trust` for PUSH and PULL
  * [#211](https://github.com/zold-io/zold/issues/211): Help nodes stay visible for longer, if they are reliable
  * [#140](https://github.com/zold-io/zold/issues/140): HTTPS for the entire RESTful API
  * [#280](https://github.com/zold-io/zold/issues/280): Pro-active pulling of wallets
  * Release [Java client API](https://github.com/zold-io/java-api)
  * [#96](https://github.com/zold-io/zold/issues/96): NScore for PULL
  * [#74](https://github.com/zold-io/zold/issues/74): Make local changes visible before PUSH
  * [#43](https://github.com/zold-io/zold/issues/43): Score graph
  * MongoDB backend
  * AWS DynamoDB backend
  * AWS S3 backend
  * Incremental HTTP protocol, to avoid traffic duplication
  * Mobile wallet (iOS and Android)
  * Mobile node
  * Launch Telegram bot

## 2018

15 Nov<br/>
[Zold-stress](https://github.com/zold-io/zold-stress), a stress-test automated
command like toolkit released.
Score calculating code moved to its own repository [zold-score](https://github.com/zold-io/zold-score),
and C/C++ implementation introduced.
Node aliases introduced in [#249](https://github.com/zold-io/zold/issues/249).
HTTP requests performance improved in [#176](https://github.com/zold-io/zold/issues/176).
Reboot mechanism is connected to the RubyGems website, in [#181](https://github.com/zold-io/zold/issues/181).

30 Jul<br/>
[#412](https://github.com/zold-io/zold/issues/412):
Scores are being calculated in a separate process, which
makes HTTP front-end a few times faster (but still pretty slow).

22 Jul<br/>
[#399](https://github.com/zold-io/zold/issues/399):
PUSH, PULL, and UPDATE are multi-threaded now, which makes
them much faster than before.

14 Jul<br/>
[#402](https://github.com/zold-io/zold/issues/402):
Critical bugs with nodes connectivity were fixed,
the network is stable (over 70 nodes).

8 Jul<br/>
This [blog](https://github.com/zold-io/blog.zold.io)
has been started and the first article has been published.

2 Jul<br/>
The first version of the
[Green Paper](https://papers.zold.io/green-paper.pdf) has been published.

16 Jun<br/>
We started to recruit Ruby developers on
[StackOverflow](https://stackoverflow.com/jobs/194602/brave-ruby-developer-for-a-new-cryptocurrency-zerocracy),
to build a team.

28 May<br/>
The first transaction has been sent.

20 May<br/>
The first version of the [White Paper](https://papers.zold.io/wp.pdf) has been published.

12 May<br/>
Version [0.1](https://github.com/zold-io/zold/tree/0.1) has been
[released](https://rubygems.org/gems/zold/versions/0.1) to RubyGems.

29 Jan<br/>
The idea was born.
It became obvious that having our own cryptocurrency would be
benefitial for [Zerocracy](https://www.zerocracy.com).


