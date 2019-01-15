---
layout: post
title: Taxes Explained
date: 2019-01-15
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Every owner of a Zold wallet has to pay regular taxes,
  in order to make sure the network maintains the wallet
  and enables outgoing payments.
tags: architecture
---

Each decentralized payment system (aka cryptocurrency) relies on
its most active participants, who maintain its hardware. The system is alive
when it has a big enough number of _nodes_, which are servers working online
24/7, communicating according to the pre-defined protocol. Zold is not
different. However, the way cryptocurrency users reward those enthusiasts
is different in Zold, comparing to <del>most</del> other cryptos. There
are no transaction processing fees. Instead, there are wallet maintenance
_taxes_. Here is how they work.

<!--more-->

BTW, an official description of taxes you can find in our
[White Paper](https://papers.zold.io/wp.pdf). This blog post is a less
formal explanation, to help you understand the concept.

The principle is simple: a wallet is in _good standing_ if its taxes
are paid. If taxes are not paid, no node in the network will accept the
wallet via PUSH operation. They will still maintain the wallet, but will
not allow it to do any modifications.

The amount of taxes each wallet has to pay depends on the age of the wallet
and the amount of transactions it contains. The larger the wallet and the
older it is, the more taxes it has to pay. Say, a one-year-old
wallet with 4096 transactions has to pay 16 ZLD of taxes. If, say, only 5 ZLD
is paid, the wallet won't be eligible for modifications---the network
will reject any attempts to modify it. Again, it doesn't mean the wallet
will be thrown away. It will remain inside nodes, but only for verification
purposes.

How does a wallet owner pays taxes?

It is done with a command line command `taxes`. First, you check how much
taxes is owed (replace `00000000000ff1ce` with your wallet ID):

{% highlight bash %}
$ zold taxes debt 00000000000ff1ce
0.05ZLD
{% endhighlight %}

The current debt is 0.05 ZLD and doesn't need to be paid as of yet. The debt
of up to 1 ZLD is tolerable and the network won't complain about it. If the
debt is larger, the network will start rejecting wallet modifications.

To pay taxes you do this:

{% highlight bash %}
$ zold taxes pay 00000000000ff1ce
{% endhighlight %}

Zold command line software will go through the network, find the best looking
and the closest nodes with the highest scores, and pick one of them, randomly.
Then, it will make an outgoing transaction from your wallet to the wallet
of that node. The transaction will contain the information of the score
of that wallet in the "details" section.

Thus, the wallet runs a "lottery" among all visible nodes and the
one that wins---gets the taxes. The maximum amount of taxes paid in one
transaction is no more than 16 ZLD. Thus, the wallet owner will run a number
of lotteries is the tax debt is more than 16 ZLD.
Since node owners are interested to win these lotteries more often, they
will try to make their servers more stable and available---this is how
Zold clients decide which nodes to work with, and that's who they will choose
to participate in the lotteries.
BTW, the numbers may
change in the future, the exact numbers are configured in
[tax.rb](https://github.com/zold-io/zold/blob/master/lib/zold/tax.rb) file.

Then, you will have to push your wallet to the network:

{% highlight bash %}
$ zold push 00000000000ff1ce
{% endhighlight %}

Now, your taxes are fully paid.

This procedure is done automatically every time you make a payment. However,
you can skip it with `--dont-pay-taxes` option:

{% highlight bash %}
$ zold pay 00000000000ff1ce 1234123412341234 \
  19.99 'For the pizza' --dont-pay-taxes
{% endhighlight %}

Unless that option is specified, the `pay` command will always check whether
the taxes are paid and will pay them if not.

Why did we invent this tax paying mechanism instead of more traditional payment
processing fees, like Bitcoin, Ethereum and many other cryptocurrencies have?
Because Zold doesn't process transactions, as Blockchain does (in blocks), but
works with wallets as solid pieces of data. It's impossible (or it would be
very expensive, time wise) to verify all transactions in a wallet and make sure
all of them were covered by processing commissions. Blockchain can do that because
the verification happens only once per a large block of transactions and never
needs to happen again. In Zold, a wallet may be verified multiple times over
the course of its lifetime, and it may contain thousands of transactions.

Thus, it is more convenient to tax an entire wallet, and do it less often than
once per each payment. Moreover, Zold is a _cryptocurrency for micropayments_,
which implies that potentiall there will be many of them in each wallet.

To check whether your wallet is in good standing, run this:

{% highlight bash %}
$ zold taxes show 00000000000ff1ce
{% endhighlight %}

The formula for taxes calculation may be changed in the future, but the taxes
you paid before will cover your wallet as promised. This means that no matter
how expensive wallet maintenance may be in the future, previous periods will
be covered by previous payments. If it was 16 ZLD per year for your wallet
and you already paid for two years, you will have two years covered even if
it will cost 32 ZLD per year for future users and younger wallets.
