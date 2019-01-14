---
layout: page
title: Table of Contents
date: 2019-01-14
author: yegor256
permalink: toc.html
description: |
  This is the full list of articles published in this
  blog; if you want to contribute, feel free to email
  us or just send a pull request.
tags: strategy
---

<article itemprop="blogPosts" itemscope="" itemtype="http://schema.org/BlogPosting">
{% for page in site.posts %}
  <p>
    <a href="{{ page.url }}">
      <strong>
        <span itemprop="name headline mainEntityOfPage">{{ page.title }}</span>
      </strong>
    </a>
    <br/>
    <span style="color:gray">
      {{ page.author.name }}
      &nbsp;
      <time itemprop="datePublished dateModified" datetime="{{ page.date | date_to_xmlschema }}">
        {{ page.date | date: "%-d %B %Y" }}
      </time>
    </span>
  </p>
{% endfor %}
</article>
