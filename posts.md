---
layout: page
permalink: /posts/
title: notebook
description: A few ideas, data, code and discussions about what I am doing
ref: post
lang: en
---

<ul class="post-list">
{% assign posts=site.posts reversed | where:"lang", page.lang %}
{% for post in posts  %}
    <li>
        <h2><a class="poem-title" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h2>
        <p class="post-meta">{{ post.date | date: '%B %-d, %Y — %H:%M' }}</p>
      </li>
{% endfor %}
</ul>


