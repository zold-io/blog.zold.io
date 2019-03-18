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
our [SDK](https://github.com/zold-io/zold-ruby-sdk),
if your programming language is Ruby or Java. Otherwise, just
work via the RESTful API and you will be fine, it's not so difficult.

<!--more-->

First, you create your own wallet at the [WTS](https://wts.zold.io).
You will get a [keygap]({% post_url 2018/07/2018-07-18-keygap %}).
Make sure you keep it safe and secure. Then,
you get the API token [here](https://wts.zold.io/api).

You should send this token in the `X-Zold-WTS` header in all HTTP requests.

## Receiving Zolds

When a customer is ready to send you zolds, you create an
[invoice]({% post_url 2018/08/2018-08-06-invoices %}), by sending a simple
HTTP request to the WTS RESTful API:

{% highlight text %}
GET https://wts.zold.io/invoice.json
{% endhighlight %}

The JSON you get in return will look similar to this:

{% highlight text %}
{ "prefix": "JjsY7sLns", "invoice": "JjsY7sLns@0123456701234567" }
{% endhighlight %}

Then, you generate two secret numbers: `R` and `T` (make sure they are long enough, at least 8 digits each).
You keep these two numbers somewhere in your database, attached to this
particular payment you are expecting from this particular customer.
You ask your customer to send the payment to the invoice you just
generated and mention number `R` in the payment details.

Then, you make an HTTP request to the Callback API:

{% highlight text %}
GET /wait-for?prefix=P&regexp=R&uri=U&token=T
{% endhighlight %}

Here, `P` is the prefix from the invoice you just generated, and `U` is
the URI where you are expecting a callback, when the payment arrives to
your wallet.

When it arrives, you will get a GET HTTP request from the WTS
and it will look like this:

{% highlight text %}
GET /?amount=A&details=R&token=T
{% endhighlight %}

Here, `A` will be the amount of zents just arrived to your wallet, `R` and `T`
will be the numbers you generated before. Using them, you can find the customer
in your database and apply the incoming payment to their balance.

## Sending Zolds

Sending is easier. You just make a single POST HTTP request, which should
look like this:

{% highlight text %}
POST /do-pay?bnf=W&amount=A&details=R&keygap=K&noredirect=on
{% endhighlight %}

Here, `W` is the ID of the wallet you are sending the payment to,
`A` is the amount to send (in zents),
`R` is some text the receiver will see in the transaction details,
and `K` is your [keygap]({% post_url 2018/07/2018-07-18-keygap %}).
If the HTTP status code is not 200, something was wrong and you will
see the error text right in the body of the response. If the response
is 200, you should get the job ID from the `X-Zold-Job` HTTP header
of the response.

Then, you should check the status of the job here, using `J` as the job ID
you just received:

{% highlight text %}
GET /job?id=J
{% endhighlight %}

If the status of the response is 200 and the body is `OK`, the payment has been
sent. If the status is 200 and the body is `Running`, it's still in processing.
In all other cases, the payment request was not successful. More details
of the problem you can get at this URL:

{% highlight text %}
GET /output?id=J
{% endhighlight %}

This request will show you the entire log of the payment request.

BTW, every time if something goes wrong, you may see the error message
in the `X-Zold-Error` HTTP header of the response. Simply put, if this
header is present, there was some error on the server.

## Using Your Wallet

Aside from sending and receiving zolds you may be interested to
check your wallet balance, see the list of transactions, download
your private RSA key and do some other things, which are provided
by the WTS API. The API is fully documented [here](https://github.com/zold-io/wts.zold.io).
If you have any questions or suggestions, don't hesitate to
[submit a ticket](https://github.com/zold-io/wts.zold.io/issues).

If something doesn't work as expected, don't hesitate to ask for help
in our [Telegram group](https://t.me/zold_io) or simply submit a GitHub
ticket [here](https://github.com/zold-io/wts.zold.io/issues).
