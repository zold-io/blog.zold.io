---
layout: post
title: How to Run Zold Node?
date: 2019-01-10
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  To join Zold network as a Hoster all you need to
  do is to rent a VPS, install Ruby, install Zold
  gem, and run it; here is how.
tags: architecture
---

Even though we have [some documentation](https://github.com/zold-io/zold#how-to-start-a-node)
in GitHub, there are many
questions from newcomers who want to join Zold networks and become
Hosters to earn [hosting bonuses]({% post_url 2018/08/2018-08-14-hosting-bonuses %})
and taxes---how to run a node? Were
is the video? Why it doesn't work for me? Who can help? Here is a short
summary of how it is supposed to work. If it doesn't, please post your
comments below, one of us will help you out.

<!--more-->

First of all, you will need a server. I'd recommend to use
a [VPS](https://en.wikipedia.org/wiki/Virtual_private_server),
instead of a dedicated one. You can get it cheap, just browse
[LowEndBox](https://lowendbox.com/), they have plenty of offers. Make sure
your VPS has at least 2 Gb of RAM. The size of the HDD/SDD
doesn't matter. It should cost you around $40 a year.
I recommend:
[Contabo](https://contabo.com/?show=vps),
[DigitalOcean](https://www.digitalocean.com),
[Vultr](https://www.vultr.com/),
and
[Linode](https://www.linode.com/pricing).

What is the best operating system? I've tried Ubuntu and CentOS. Ubuntu seems
easier to configure and more stable. But it's a matter of taste.

Then, you need to install Ruby 2.5+. Here is our
[installation manual](https://github.com/zold-io/zold/blob/master/INSTALL.md).
When it's installed, you should be able to do something like this:

{% highlight bash %}
$ ruby -v
ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-linux]
{% endhighlight %}

If something goes wrong, ask for help in
[StackOverflow](https://stackoverflow.com) or in our
[Telegram group](https://t.me/zold_io).

Now it's time to install Zold. It should be as simple as this:

{% highlight bash %}
$ gem install zold
$ zold -v
0.21.4
{% endhighlight %}

If it doesn't work, submit a ticket to our
[GitHub repo](https://github.com/zold-io/zold). Copy everything you see
in the console and paste it to the ticket. It is very possible that our
software has bugs, it's still in the very early experimental stage.

Once Zold is installed, I would suggest you to to go [WTS](https://wts.zold.io)
and create a wallet there. Then, go to the [Invoice](https://wts.zold.io/invoice) page there and get
your [invoice]({% post_url 2018/08/2018-08-06-invoices %}).

Now you are ready to join the network! Run this:

{% highlight bash %}
$ zold node --nohup --invoice=<your invoice>
{% endhighlight %}

Your node should be up and running. It will be visible in your IP address
via the port 4096 (you will need to open that port for inbound connections,
if you have a firewall). To check whether your node is alive,
"open" it in a browser. It should look similar to this page:
[b2.zold.io:4096](http://b2.zold.io:4096/])

Now, wait for a few hours and check the presence of your node in
our network monitoring page: [/health](http://www.zold.io/health.html).
If it's not there and the score of your node is above 4, ping us in
[Telegram](https://t.me/zold_io), maybe there is something wrong.

Once you see your node up and running, get ready to get some hosting bonuses
and taxes. Regularly check your wallet in [WTS](https://wts.zold.io).

You may also want to subscribe to our [Telegram channel](https://t.me/zold_wts),
where WTS posts system messages about the network. They may help you understand
what's going on under the hood.

Thanks for supporting [Zold and Zerocracy]({% post_url 2018/07/2018-07-08-mission %})!
