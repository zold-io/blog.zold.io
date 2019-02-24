---
layout: post
title: Nodes Discovery Protocol
date: 2018-12-28
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Zold is a decentralized system, where nodes
  discover each other semi-randomly, using HTTP
  RESTful API.
tags: concept
---

[Zold](http://www.zold.io) is a decentralized system, which specifically means that there is no
central directory of nodes, where a new node may lookup and find where
to connect to (aside from a short list of
[master nodes]({% post_url 2018/12/2018-12-27-masters-and-edges %}), of course).
The entire network may include tens of thousands of nodes,
which may be up or down at any particular moment. There is no
guarantee that any of them will be responsive or will even stay online the next
minute. How does a node finds its place in this "chaos"? How do they know
where others are located and which neighbours they can rely on?

<!--more-->

Node discovery protocol in Zold is pretty simple. Nodes "talk" to each other
via a rather primitive RESTful API, described in the
[White Paper](https://papers.zold.io/wp.pdf). They send GET and PUT requests
to each other in order to either FETCH or PUSH wallets content.

Every time an HTTP request arrives to a node, the node checks the existence of
`X-Zold-Score` header. If the header is present and its value contains
a valid score, the node adds the requestor to its own list of remote nodes.
In other words, every time a new neighbour talks to a node, its coordinates
are recorded.

Then, the node talks to its neighbours and tracks their responsiveness and
availability. Every failure is recorded, like: no connection, absence of a
wallet, slow response, etc. The least reliable neighbours are regularly removed from
the list, in order to keep the list short enough (usually around 16 addresses).

In order not to stay alone, each node performs a "reconnection" procedure
every minute. It simply goes through the list of known neighbours and asks
them, via the `/remotes` API entry point, what remotes nodes they recommend.
They return their lists and the node updates its own list.
This reconnection procedure helps keep the network connected well enough. Nodes
are never lost.

There is one more thing that is important to mention. When a node receives
a request with the `X-Zold-Score` header inside, it pays attention only
if the value of the score is 4 or above. Otherwise, the remote node is not
added to the list of neighbours. Thus, when you just start a node
and no other nodes know about it yet, you have to wait for a few hours,
until your node gains the required score. After that, the information about
your node will be spread over the network and, provided your node is
reliable enough, will not be forgotten.

Once your node is known to others, you will see it in the
[/health](http://www.zold.io/health.html) page.

