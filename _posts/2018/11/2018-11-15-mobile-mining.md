---
layout: post
title: Mining on Mobile Phones
date: 2018-11-15
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Thanks to the lightweight nature of Zold architecture
  it's possible and advisable to maintain low-cost nodes,
  including mobile phones, pads, TV sets, and even watches.
tags: architecture
---

[Proof-of-Work](https://en.wikipedia.org/wiki/Proof-of-work_system)
is the core principle behind many Blockchains, including
Bitcoin and Ethereum. The idea behind it is simple: a node, in order to
win the game and earn something, has to compete with _all other_ nodes.
There is only one thing that guarantees success in the competition: its
CPU power. The faster and more expensive the computer, the higher
the chances that it will find the [nonce](https://en.bitcoin.it/wiki/Nonce) for the next block.

<!--more-->

There are over [9,000](https://coin.dance/nodes) "nodes" in Bitcoin at the moment.
They close [144 blocks](https://www.bitcoinmining.com/what-is-the-bitcoin-block-reward/)
every day. Each closed block gives the founder of the nonce exactly 12.5 BTC.
Each node is a group of pretty expensive [ASICs](https://en.wikipedia.org/wiki/Application-specific_integrated_circuit),
and it has to compete against all other nodes. In total, there are over three million
machines in the network. To help each other,
and increase chances, node owners "pool" their resources. The overall
cost of all Bitcoin nodes at the moment is already over
[$500m](https://www.reddit.com/r/Bitcoin/comments/5zxn7c/what_is_the_total_cost_of_all_the_bitcoin_mining/).

It seems that this _arms race_ is a predictable result of a
[central ledger](https://www.coindesk.com/edward-snowden-public-ledger-is-bitcoins-big-flaw).
Every node is trying to get into the ledger with its own block, and only
one of them succeeds. Low-cost equipment, like a commodity server
or a laptop, won't stand a chance. The hardware has to be expensive. Not just
expensive, but _more_ expensive than the other nodes.

[Zold](https://www.zold.io), even though it also relies on the same Proof-of-Work principle, doesn't
have a central ledger. Each wallet has its own list of transactions and there
is no reason for nodes to compete with the entire network in order to earn
fees (known as "taxes"). With Zold they compete with just a few
of their neighbour machines. Who gets the reward is decided by the paying
wallet owner.

Thanks to this significant architectural advantage, Zold nodes can be, and will
be, hosted on low-cost hardware, starting from cloud 2-4 CPUs virtual servers
down to mobile phones, TV sets, and even watches.

Instead of wasting the planet's resources on expensive equipment dedicated solely
to transaction processing, Zold is offering a much wiser and greener alternative:
use existing hardware, including our mobile phones and tablets. They're not doing
anything important most of the time anyway.

Aside from this, with many millions of
devices, it will be almost impossible to 51%-attack the network.
There are no mining pools in Zold and no centralized control of resources.
Consequently, Zold is the only truly decentralized and distributed cryptocurrency,
where wallet owners are the real owners of their digital money.

