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
{% for post in site.posts reversed %}
    <li>
        <h2><a class="post-title" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h2>
        <p class="post-meta">{{ post.date | date: '%B %-d, %Y — %H:%M' }}</p>
      </li>
{% endfor %}
</ul>



