---
layout: post
title: Response Time is Crucial
date: 2018-10-31
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  GET and PUT HTTP requests in Zold network must be fast
  enough to guarantee the accuracy and trustworthiness of data.
tags: architecture
---

We've been having technical issues recently and they all were caused
by one serious problem---our nodes were not _responsive_ enough. This
literally means that they were not responding to HTTP requests as fast
as it would be required for the network to function correctly. Why
response time is important and what happens when nodes get slow?
Here is my post-mortem analysys. Yes, the problem is gone and now it's
time to analyze why the speed of data delivery is so crucial for Zold.

<!--more-->

As you probably know from our [White Paper](https://papers.zold.io/wp.pdf),
there are three basic operations in Zold: fetch, push, and merge. The first
two (fetch and push) interact with the network of distributed nodes, the last one
(merge) works locally, putting wallet files together and
[deciding]({% post_url 2018/08/2018-08-03-consensus %}) which
set of copies is the most trustable.

Zold network is decentralized and this means, first of all, that it
consists of anonymous nodes---the servers we can't trust. We can't trust
their data and we can't trust their
[responsiveness](https://en.wikipedia.org/wiki/Responsiveness). If they are too slow
to respond, we can't wait and slow down the entire fetch or push operation.
We have to move on and ignore any particular node, which is too slow. To fetch
a wallet from the network we have to make a number of HTTP GET requests to
a number of nodes. All of them or most of them will return the content
that is merged later into the target wallet file.

Each HTTP request has a timeout parameter associated with it.
If a request takes longer than that
number, it gets terminated and its results are ignored. This is done in order
to isolate good nodes from bad ones and ensure that the speed of the entire
operation is high enough.

If just a few nodes are slow, while the rest are fast, it's not a big deal. We
still get the data we need
[fast enough]({% post_url 2018/07/2018-07-30-how-fast-is-zold %}) and we still can use them for the
merge operation, since we can compare the scores received and decide
which set of copies dominate.

However, if most of the nodes are slow, the entire network collapses. We simply
can't make any reasonable decisions about the data in the wallets, because
we reject the majority of copies coming from the network. We reject too
often and that affects the accuracy of data. Not just the
[speed]({% post_url 2018/07/2018-07-30-how-fast-is-zold %}), but the
accuracy and _trustworthiness_ of information in the wallets!

When too many nodes are too slow, they look dead to most of its clients and
other nodes. If they are dead, the information they provide can't be trusted.
If the information can't be trusted, we can't merge wallets anymore with
any reasonable guarantee.

Thus, it's absolutely critical for Zold to make sure its nodes are fast enough.
At the moment here is the data I'm getting from one of the nodes:

{% highlight text %}
$ ab -n 1000 -c 10 http://b2.zold.io:4096/wallet/0000000000000000
This is ApacheBench, Version 2.3 <$Revision: 1826891 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking b2.zold.io (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        thin
Server Hostname:        b2.zold.io
Server Port:            4096

Document Path:          /wallet/0000000000000000
Document Length:        4126 bytes

Concurrency Level:      10
Time taken for tests:   19.004 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      4640190 bytes
HTML transferred:       4126000 bytes
Requests per second:    52.62 [#/sec] (mean)
Time per request:       190.043 [ms] (mean)
Time per request:       19.004 [ms] (mean, across all concurrent requests)
Transfer rate:          238.44 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:       45   53   4.9     52      99
Processing:    72  135  45.4    123     403
Waiting:       71  134  45.4    122     403
Total:        122  188  46.3    176     465

Percentage of the requests served within a certain time (ms)
  50%    176
  66%    193
  75%    205
  80%    215
  90%    250
  95%    279
  98%    321
  99%    361
 100%    465 (longest request)
{% endhighlight %}

As you see, the majority of requests finish in less than 200 milliseconds.
This is good enough for Zold network to function correctly. We will definitely
aim for higher speed in the future, but for now these numbers are good enough.
