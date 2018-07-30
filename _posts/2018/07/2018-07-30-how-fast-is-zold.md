---
layout: post
title: How Fast Is Zold?
date: 2018-07-30
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Zold claims to be a low-latency cryptocurrency; in this
  article I make an attempt to analyze how theoretically fast
  it is.
tags: speed
---

[Zold](https://www.zold.io) is a non-blockchain cryptocurrency without a central ledger.
Each Zold wallet has its own list of transactions, both positive (coming in)
and negative (coming out). Two wallets take participation in each payment.
The first wallet gets a money spending transaction and the second one
gets a money receiving transaction. In order to spread the knowledge
about a new payment both wallets get distributed to as many nodes as possible.
The question is how long will it take for the entire network to accept
new payments, if they are coming in at high speed? What will be the so called
"confirmation time"?

<!--more-->

Nodes are randomly connected to each other.
Each node maintains a list of _R_ neighbour nodes.
_R_ may unpredictably differ from zero up to _N_,
the total number of nodes in the network.

When a wallet modification is pushed to a node, the node
pulls the wallet from its neighbours, merges the changes, and pushes
the wallet back to its visible neighbours. Each of them will do the same, until
the entire network has a new version of the wallet.
How many _hops_ will the wallet have to jump through until the process is complete?

It seems that _H_, the approximate number of hops,
will be equal to the logarithm of _N_ to base _R_:

<p style="text-align:center">
<img src='http://latex.codecogs.com/svg.latex?H%3Dlog_RN'
  alt='equation' class='equation'/>
</p>

Now we have to find out how fast each node will be able to do those pull,
merge, and push operations for each wallet. It seems logical to assume that
the speed of the entire update will depend on two factors:
how fast a neighbour node responds and how many of them are there. With the
current [Ruby]({% post_url 2018-07-20-why-ruby %}) software,
utilizing 4 parallel threads, an average response
time _P_ is 400ms (which is pretty high and has to be decreased
down to 50ms in future versions). With this response time and the current
average _R_ of 40 nodes, the speed of update _S_ is 50 seconds.
We can assume that the dependency is linear:

<p style="text-align:center">
<img src='http://latex.codecogs.com/svg.latex?S%3DP%5Ctimes%7B%7D%20R%20%5Ctimes%7B%7D3'
  alt='equation' class='equation'/>
</p>

Thus, the network of _N_=1000 nodes, where each node is connected to _R_=16 neighbours,
can accept a modification of a single wallet and spread it over all nodes
in approximately 48 seconds (confirmation time):

<p style="text-align:center">
<img src='http://latex.codecogs.com/svg.latex?T%3DH%5Ctimes%7B%7D%20S%3Dlog_{16}{1000}%5Ctimes%7B%7D%20400ms%5Ctimes%7B%7D%2016%5Ctimes%7B%7D%203%3D48s'
  alt='equation' class='equation'/>
</p>

The amount of transactions a single modification contains doesn't seriously
affect the result of the formula, since the most time consuming part of the update
operation is the transfer of data between nodes. With 1,000,000 nodes in the
network and the same _R_ of 16, the confirmation time will be 90 second.
Pretty fast, huh? As you see, the size of the network has almost no effect to the speed
of data propagation.

What will indeed slow down the propagation process
is the length of the queue of wallets each
node maintains. When a node receives a new version of a wallet, it doesn't
start working with it immediately, but places it into the queue. The longer
the queue, the bigger the value of _S_, which means slower processing of each
update.

To resolve this issue each node accumulates wallets in the queue and
pushes them to the neighbours in packages. Also, it pulls packages of wallets
from its neighbours. The length of the package _L_ depends on _F_, the intensity
of the wallets flow:

<p style="text-align:center">
<img src='http://latex.codecogs.com/svg.latex?L%3DF%5Ctimes%20S'
  alt='equation' class='equation'/>
</p>

With the current _S_ of 50 seconds and the insentity _F_ of a thousand wallets per second,
the approximate length of the package will be 50,000 wallets:

<p style="text-align:center">
<img src='http://latex.codecogs.com/svg.latex?L%3D1000w%2Fs%20%5Ctimes%2050s%20%3D%2050000w'
  alt='equation' class='equation'/>
</p>

Obviously, the length of the package _L_ will affect its processing time _S_.
The larger the package, the longer it takes to process its content and to
deliver it over the network.
The delivery process is the largest time consumer.
The existing Ruby software spends up to 10μs out of the entire 50 seconds of _S_ for the merge of the wallet,
which approximately is 0.00002% of _S_.
The rest of the time is spent on the communication with its neighbours over the HTTP protocol.

The time it takes to deliver a package over HTTP consists of
1) connection time, 2) data transfer time, and 3) HTTP processing time on the node.
The connection takes 20-50ms. The data transfer takes 10μs per transaction,
if the speed of the network is 100Mb/s and an average size of a single transaction is 1Kb.
The largest time consumer is the HTTP processing code inside each node, which
doesn't change significantly, no matter how large is the package.

Ergo, the value of _P_ for 50,000 wallet modifications with a single transaction
in each one, would be equal to:

<p style="text-align:center">
<img src='http://latex.codecogs.com/svg.latex?P%3D50ms+50000%5Ctimes%200.01%5Cmu%20s+350ms=900ms'
  alt='equation' class='equation'/>
</p>

To summarize, even though the confirmation time depends on the amount of
nodes in the network and the intensity of modificiations made by wallet
owners, the dependency is not linear. The confirmation time does grow, but
even with a million nodes in the network and a thousand transactions per second
its value jumps from 20 to 200 seconds. With a million transactions
per second and a million nodes in the network the estimated confirmation time
will be equal to an hour.

To compare, VISA's peak volume was
[47,000 tps](https://www.visa.com/blogarchives/us/2013/10/10/stress-test-prepares-visanet-for-the-most-wonderful-time-of-the-year/index.html)
in 2013, while its [regular volume](https://usa.visa.com/run-your-business/small-business-tools/retail.html)
is 1,700 tps.
This [analysis](https://www.abitgreedy.com/transaction-speed/)
demonstrates how fast some other cryptocurrencies are.

_We are developing a set of stress tests at the moment, in order
to confirm the numbers and formulas presented above._
