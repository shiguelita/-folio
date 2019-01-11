---
layout: page
permalink: /notebook_pt/
title: notebook
description: Algumas idéias, dados, códigos e discussões sobre o que estou fazendo
ref: post
lang: pt
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



