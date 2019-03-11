---
layout: post
title: How to Integrate
date: 2019-03-11
author:
  name: Yegor Bugayenko
  twitter: yegor256
description: |
  Your online shop, a mobile app, or a crypto exchange
  may accept and send ZLD as payment method, here is how
  you integrate with our API.
tags: architecture
---

Your online shop, a mobile app, or a crypto exchange may accept or send
zolds to your customers, via our hosted web API. Of course, you can implement
it all yourself, without the API, but we made it for you, to make your
life easier. Here are the step-by-step instructions. You can also use
our SDK, if your programming language is Ruby or Java. Otherwise, just
work via the RESTful API and you will be fine, it's not complex.

<!--more-->

First, you create your own wallet at the [WTS](https://wts.zold.io).
You will get a [keygap]({% post_url 2018/07/2018-07-18-keygap %}).
Make sure you keep it safe and secure. Then,
you get the API token [here](https://wts.zold.io/api).

You should send this token in the `X-Zold-WTS` header in all HTTP requests.

Then, when a customer is ready to send you zolds, you create an
[invoice]({% post_url 2018/08/2018-08-06-invoices %}), by sending a simple
HTTP request to the WTS RESTful API:

{% highlight text %}
GET /invoice.json
{% endhighlight %}

The JSON you get in return will look similar to this:

{% highlight text %}
{ "prefix": "JjsY7sLns", "invoice": "JjsY7sLns@0123456701234567" }
{% endhighlight %}

Then, you make an HTTP request to the Callback API:

{% highlight text %}
GET /wait-for?prefix=B&regexp=C&uri=D&token=E
{% endhighlight %}

Here, `A` is your wallet ID you get from

Give this information to the customer, he/she will know what to do with it.
