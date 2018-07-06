---
layout: page
permalink: /notebook/
title: notebook
description: A few ideas, data, and discussions about what I am doing
---

<ul class="post-list">
{% for poem in site.notebook reversed %}
    <li>
        <h2><a class="poem-title" href="{{ poem.url | prepend: site.baseurl }}">{{ poem.title }}</a></h2>
        <p class="post-meta">{{ poem.date | date: '%B %-d, %Y — %H:%M' }}</p>
      </li>
{% endfor %}
</ul>