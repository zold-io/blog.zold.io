---
layout: post
title: Garbage Collection
date: 2018-12-26
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  The mechanism of garbage collection helps nodes keep their
  data optimized, removing unnecessary empty wallets which
  are not needed by anyone.
tags: concept
---

The mechanism of garbage collecting was recently
[introduced](https://github.com/zold-io/zold/issues/622) in
our server-side software. It's simple: the node automatically
removes all empty wallets, which are older than 10 days. What is the
purpose of this process and what are its possible negative consequences?
There are some, let me show you.

<!--more-->

An empty wallet is a plain text file with an ID and a public RSA key
inside. It was created by its owner and pushed to the network. There are
no transactions inside. No payments have been made to the wallet and,
of course, no payment have been sent out of it. The node maintains
that wallet for some time and then, when it's obvious that nobody is
interested in it anymore, deletes it.

Why? Well, mostly in order to optimize disk space usage. Such a wallet
is just a waste.

What happens if a payment arrives to this wallet after it's deleted? Say,
Jeff creates a wallet, pushes it to the network and gives an
[invoice]({% post_url 2018/08/2018-08-06-invoices %})
to Jessica. She delays a payment for a few months and then, all of
a sudden, sends it to Jeff from her own wallet.

Jeff pulls his wallet and the network says: File not found. However, the payment
is still in the network, in Jessica's wallet. It's not lost. It's just Jeff's
wallet is lost. What should Jeff do in order to receive the money?

He should just push his wallet again to the network. Nodes will receive his
wallet and will propagate Jessica's payment to it. In other words, each node
will fill up Jeff's wallet, when it arrives, with the payments all other wallets
have for it.

What will happen if Jeff loses his wallet after the network "garbage collects" it?
He will not be able to push it to the network again and the payment will never
arrive to it, right? Correct. If Jeff loses it, it's lost forever. Jessica's
wallet will still have that negative transaction, but Jeff will never receive it.
The money will be lost forever.

PS. Well, actually, if Jeff still has his private RSA key and remembers
the ID of the wallet, he can restore it and push to the network.
