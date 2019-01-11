---
layout: page
permalink: /notebook/
title: notebook
description: A few ideas, data, code and discussions about what I am doing
ref: post
lang: en
---

<ul class="post-list">
{% assign posts=site.posts | where:"lang", page.lang %}
{% for poem in site.notebook reversed %}
    <li>
        <h2><a class="poem-title" href="{{ poem.url | prepend: site.baseurl }}">{{ poem.title }}</a></h2>
        <p class="post-meta">{{ poem.date | date: '%B %-d, %Y — %H:%M' }}</p>
      </li>
{% endfor %}
</ul>



