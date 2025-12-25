---
layout: post
title: How Trustable Is Blockchain?
date: 2018-07-25
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Blockchain is one of the solutions for a decentralized
  digital payment problem; how trustable it is and what
  are the important components of trust?
tags: blockchain
---

It seems that the concept of trust in Blockchain is not understood clearly enough.
Most of us believe that Blockchain is an "immutable ledger,"
but how trustable it is and what is the difference between Blockchain and, for example,
MySQL---are the questions usually left without explicit answers.

<!--more-->

In a _centralized_ data storage, also known as "database," the owner of the
hardware and the software is responsible for the consistency of data. If it
is a bank, where you have an account, its programmers and server administrators
keep your information private, in the server(s) they maintain. You trust
that they won't destroy the data and won't modify them without
your permission. It is called "centralized" because there is a single
point of control---a center.

If the center makes a decision to steal your money, nothing will stop them.
One day you wake up and your bank account is empty. The good news
is that you will know exactly who's stolen it and will be able
to use legal forces to get them back.

Thus, you trust the consistency of the account data, as long as you trust
the owner of the bank and the power of the police.

A _decentralized_ data storage works differently. It has a big amount of
servers, managed by different programmers or groups of them
([9566](https://bitnodes.earn.com/) in the Bitcoin network at the time of writing). Each server
contains a copy of your bank account. Each server knows that you have, say,
a hundred dollars.

You don't trust any of them individually, since you don't know who they are. Those servers
belong to anonymous organizations or people. You won't be able to use any
legal power against them if they steal your money. They are nobodies. Moreover, they
never promised you to keep your data safe. There is no legal contract between
you and them.

How can we trust such a group of servers,
if we don't trust any of them individually?

This is the problem Bitcoin and other cryptocurrencies, like [Zold](http://www.zold.io), solve.

They all invent a protocol, which every single server is supposed to use and follow.
They use the protocol to constantly exchange the information about
user accounts and when/if any server starts lying, all other servers ignores
it or even eject from the group.

Since it is assumed that the majority of servers are not lying, you
can trust them as a group.
We simply believe that the idea to steal your money won't hit 51% of all server
owners. Some of them may want to do that time to time, but since they always
will be in minority, all other servers will suppress their illegal attempts,
just like it works in a democratic voting system.

This "majority vs. minority" protocol, also known as "consensus protocol," is
the core of any cryptocurrency. Blockchain is one of those protocols and the
most popular at the moment.

Hence, the bigger the amount of server owners,
the more trustable is the cryptocurrency. The amount of servers is not
so important. What is important is the amount or people and organizations
involved. The cumulative trust of the group is a derivative of the amount
of conflicts they have between each other. FYI, there are
about [ten major players](https://www.blockchain.com/en/pools)
in the Bitcoin network at the time of writing.
