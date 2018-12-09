---
layout: post
title: Stress Tests
date: 2018-11-22
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Here is a simple stress test created to prove the
  robustness of Zold network under heavy load; you
  can try it yourself.
tags: architecture
---

It was promised [earlier]({% post_url 2018/07/2018-07-30-how-fast-is-zold %})
that Zold cryptocurrency is way faster than any other Blockchain-based
solution can even dream of. We claimed that a million transactions per second
is not the limit. Theoretically it's true, because there is no centralized
ledger in Zold. Each node maintains its own list of wallets. Thus, the bigger
the number of nodes, the faster the network. We created a simple tool to
prove our claims.

<!--more-->

It's an open source command line Ruby gem called [zold-stress](https://rubygems.org/gems/zold-stress),
which you install just like you install [zold](https://rubygems.org/gems/zold):

{% highlight bash %}
$ gem install zold-stress
{% endhighlight %}

Then, you create a directory and pull a wallet there, with some ZLD on board.
It has to be your wallet, obviously. And you have to have a private key for it:

{% highlight bash %}
$ mkdir stress
$ cd stress
$ zold pull 2deb903f1fa1ea68
{% endhighlight %}

Then, you run `zold-stress`:

{% highlight bash %}
$ zold-stress --rounds=4 --batch=16 --pool=8 --threads=8
{% endhighlight %}

Run it and see what happens. If it doesn't work and you see a stacktrace,
submit it to the GitHub repo, we will take a look. However, I'm pretty sure
it will work and you will see something similar to this output:


This video actually demonstrates a stress test of a demo network, not a real
one. There are eight nodes and they are all hosted locally. The test is
running from my laptop. It doesn't show a million transactions per second,
but.

The arguments of the tool mean the following:

  * `--batch`: the number of transactions to send in each round;
  * `--rounds`: the number of rounds;
  * `--threads`: the number of parallel threads to use;
  * `--pool`: the number of wallets to send payments from.

The next step for us is to grow the network up to 300+ nodes and run the
same test from multiple entry points. The goal is to demonstrate the speed
of 10,000 tps.
