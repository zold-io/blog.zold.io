---
layout: post
title: Zold Consensus
date: 2018-08-02
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Zold has its own consensus protocol, which guarantees
  that the data, backed by the majority of computation power,
  always dominates.
tags: consensus
---

Any distributed database that consists of anonymous zero-trust servers
must have a certain [consensus](https://en.wikipedia.org/wiki/Consensus_%28computer_science%29)
protocol, which guarantees that conflicts between
different versions of the same data are resolved in favor of a dominating part
of the network. Zold has its own consensus protocol, which is different from
what Blockchain uses. However, Zold, just like Blockchain, relies
on the proof-of-work principle.

<!--more-->

Each wallet is a plain text file with a list of transactions. Each transaction
is a line in the file. The format of the line is described in the
[White Paper](https://papers.zold.io/wp.pdf). In order to make a payment,
a user creates a new line with the information about the amount and
the beneficiary, signs it with the private [RSA](https://en.wikipedia.org/wiki/RSA_%28cryptosystem%29) key, and adds the line
to the end of the file. Then the user _pushes_ the wallet to a few nodes.

The node receives the new version of the wallet file, validates the RSA
signature, and, if it matches, overwrites the existing file with a new one.
Then, the node pushes the file to all nodes it is aware of. They do exactly
the same and [eventually]({% post_url 2018-07-30-how-fast-is-zold %})
the new version of the file is present in every node of the network.

Sounds like a simple scenario, but let's see what will happen if a user
tries to abuse the system. For example, he may create two versions of the wallet. In one
version he will send all money to Jeff, while in another version
he will send all money to Mary. Simply put, he will spend all of
his money twice, which obviously is an illegal operation. Then, he will
push the first version of the wallet to one node and the second version
of the wallet to another node.

Both nodes will validate those payments and will think that they are valid,
since RSA signatures match and the wallet has enough money. Both nodes
will think that they got a hold of a perfectly legal transaction.
Then, they both will try to "convience" their neighbour nodes that the payment
was correct by pushing the wallet to them.

Eventually, some node will receive two versions of the same wallet and won't
be able to accept them both. It won't be able to send all money both to Jeff and Mary.
This is known as a [double-spending problem](https://en.wikipedia.org/wiki/Double-spending).

The node will have to make a decision, which transaction is legal, and which
one is fraudulent and has to be rolled back. A consensus protocol helps Zold nodes
make that decision. The node in conflict simply compares the amount of nodes
both wallets came from and selects the version that came from a _larger_ part
of the network.

In order to determine which part of the network is larger, Zold protocol requires
its nodes to _prove_ their sizes in terms of computation power. Each node
has to do some work and spend some time in order to calculate a _trust score_
and demonstrate it to other nodes. It takes time and CPU power in order
to find these scores. The faster the node, the higher the value of the score.
This approach is also known as [proof-of-work](https://en.wikipedia.org/wiki/Proof-of-work_system).

The part of the network is considered larger, if the cumulative score
of its nodes is bigger. By comparing cumulative scores of wallet versions,
the node in conflict makes a decision which transaction to trust.


