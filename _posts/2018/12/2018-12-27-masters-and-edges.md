---
layout: post
title: Masters And Edges
date: 2018-12-27
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Master nodes existed in Zold network from the first
  day of its existence, however only recently PUSH
  and FETCH operations started to pay attention to their
  special status.
tags: concept
---

Any decentralized system, no matter whether it's a cryptocurrency like
Bitcoin or Ethereum, or something else like
[Tor](https://en.wikipedia.org/wiki/Tor_%28anonymity_network%29),
[Kazaa](https://en.wikipedia.org/wiki/Kazaa),
[BitTorrent](https://en.wikipedia.org/wiki/BitTorrent),
[DNS](https://en.wikipedia.org/wiki/Domain_Name_System),
or
[Skype](https://en.wikipedia.org/wiki/Skype),
need some amount of servers with a special level of trust. When new clients
connect to the network they have to interact with _some_ servers they
can fully trust. Then, those "nodes" can redirect the clietn to other
places. Something similar existed in Zold from the first day, but got
a special name just a week ago.

<!--more-->

[Bitcoin](https://bitcoin.org/en/)
has seven entry points (at the time of writing hard-coded in
[`chainparams.cpp`](https://github.com/bitcoin/bitcoin/blob/master/src/chainparams.cpp)).
They are owned by Pieter Wuille, Matt Corallo, Luke Dashjr, Christian Decker,
Jonas Schnelli, Peter Todd, and Sjors Provoost. When a new Bitcoin node starts,
it discovers the network through one or a few of those seven "master" nodes.

[Ethereum](https://www.ethereum.org/)
has ten "boostrap" nodes (at the time of writing hard-coded in
[`mainnet_bootnodes.json`](https://github.com/ethereumproject/go-ethereum/blob/5291a2863d27e68310722e2ab9facff438e23cd8/core/config/mainnet_bootnodes.json)),
which were
[introduced](https://github.com/ethereumproject/go-ethereum/issues/4) in the first days of
their project. Who owns them, I'm not sure, the file doesn't disclose their names.

<iframe width="560" height="315" src="https://www.youtube.com/embed/8Lr0cFhwE6M" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

It is inevitable. There is no other way to build a decentralized system.

In other words, true decentralization is a myth.

We've always had the same in Zold. At the moment we have six "master" nodes, which are hard-coded
in [`masters`](https://github.com/zold-io/zold/blob/master/resources/masters) file
in our main GitHub repository. Those nodes are maintained by myself at the moment.
The list will be updated in the future, when we get more volunteers, who will be ready
to provide their equipment and we will be able to trust they will always run the software
we create, without modifications.

Just recently, a week ago, we [added](https://github.com/zold-io/zold/issues/633)
a new feature to our command line client: intolerance for _no-masters_ FETCH
and PUSH operations. Simply put, your attempt to fetch a wallet from the
network will fail if no master nodes took participation in it. All other nodes,
which are not masters (and they are obviously in majority) we call "edges."
Thus, if only edges are returning the wallet, it can not be trusted.

This feature seriosly increases trust in the entire system.

I believe, we will need more master nodes in the future, when the amount
of wallets and transactions will grow.

And, by the way, any client can disable this additional trust check, by using
this newly introduced command like argument: `--tolerate-edges`.
