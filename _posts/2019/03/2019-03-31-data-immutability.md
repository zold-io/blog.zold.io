---
layout: post
title: Immutability Guarantee
date: 2019-03-31
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  The baseline feature we use during merge operation
  is the key element of our consensus protocol;
  here is how it works.
tags: architecture
---

As you know, there is no [Blockchain](https://en.wikipedia.org/wiki/Blockchain)
in [Zold](https://www.zold.io). However, just like [Bitcoin](https://www.bitcoin.org)
and its clones, Zold also guarantees immutability of its data, up to
a certain limit. How exactly does it work? There is a
[consensus protocol]({% post_url 2018/08/2018-08-03-consensus %}),
which makes sure the majority of nodes wins when
[double spending](https://en.wikipedia.org/wiki/Double-spending)
data conflict shows up. Here is how it works, on a more technical
level.

<!--more-->

Everything happens in the _merge_ operation, but let's start from the beginning.

First, the wallet is _pulled_
from a number of nodes, each retrieved copy is stored in its own file, and
duplicates are aggregated. In the end, we have something like this
(just run `zold pull --verbose` with your wallet ID and you will
see something similar):

{% highlight text %}
#1: 89 7n 0000000000000000/-132096.0000/3t/bd24ac/2Kb 2Kb/2.32s master
#2: 13 1n 0000000000000000/-150890.0000/3t/f9e667/2Kb 2Kb/0.43s
{% endhighlight %}

Each line describes a copy of the wallet
you have locally on your disc: `#1` in the first line is the unique name of the copy;
`89` is the accumulative score of all nodes, which provided
exactly this version of the walle; `7n` is the total amount of them---there were
seven nodes. The information that goes after that is more obvious, I believe.
It's the ID of the wallet, the balance, the number of transactions, the
hex digest, and the size of the file. At the end of the line you see
`master`, which is an indicator of presence of at least one
[master node]({% post_url 2018/12/2018-12-27-masters-and-edges %})
in the list of those seven.

The second line demonstrates that one node with the score of 13 provided a
different version of the same wallet. As you see, there were also
three transactions inside, but the balance was different. A possible
[double spending](https://en.wikipedia.org/wiki/Double-spending)
problem is right in front of us!

Now it's time to _merge_ these two copies into one. There are two types
of transactions: incoming and outgoing. The merging algorithm goes
through all avaiable copies, starting with the most "scored" one,
and decides what to do with the transactions found. Outgoing transactions
get into the resulting copy with a simple validation: their RSA signatures
must match the public RSA key of the wallet. In other words, if the
owner of the wallet has singed the transaction, we trust it. That's it.

Of course, if you signed too many of them, and your total balance of the
wallet after we merge them all together is negative, the entire merge
operation fails.

A bigger question is what to do with incoming payments. They show up
in the wallet without RSA signatures, since they are signed in their own
wallets. We may go to those wallets and validate RSA signatures there
in order to make sure that they are truly valid. However, when we go
there and try to validate those wallets, we will have to _fetch_ and _merge_
them too. We will have to run a similar merging algorithm there, and it may
take a lot of time. Moreover, we may go into a recursion,
if wallets are interconnected.

In other words, a total validation is technically impossible. What do we do?

We trust the _baseline_. We treat the first copy in the list as a fully
trusted baseline and we don't validate what's in it. We just take it
as is and apply all other copies on to of it. We validate their
incoming transactions by doing fetch and merge of their relative wallets.
Of course, in each sub-operation we also treat most scored copy
as a baseline.

This baseline mechanism can be turned on by `--no-baseline` option in the
merge operation. However, as explained above, this may be very time
consuming or even technically impossible. Do it at your own risk.

Ah, one more thing. In order to protect us against "baseline attack"
we treat the first copy as a baseline only if it comes from at least
one [master node]({% post_url 2018/12/2018-12-27-masters-and-edges %}).
This feature can be turned off with `--edge-baseline`,
but it's very risky. A baseline attack can be made by a single wallet
with a large score (which it can accumulate in advance, while preparing
for the attack). An attacking node simply exposes a corrupted version of
the wallet and all other nodes will trust it. This won't happen if we
baseline only from
[master]({% post_url 2018/12/2018-12-27-masters-and-edges %}) nodes.

Now, let's get back to the immutability question. Is it possible to modify
a wallet after its money has already been spent? In order to do that
an attacker has to convince other wallets that they should treat
a new version of the wallet file as a baseline, trust it, never validate
its incoming transactions, and blindly accept it. This may only happen if a fake master node
will be speaking with a dominating amount of score.

Thus, Zold is immutable as long as
[master]({% post_url 2018/12/2018-12-27-masters-and-edges %}) nodes run
legal software and don't manipulate the score.

