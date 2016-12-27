---
layout: page
title: poetry
permalink: /poetry/
---


  <ul class="post-list">
    {% for post in site.tags.Poetry %}
      <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
      <p>
      <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.content }}
      </a>
      </p>
      </li>
    {% endfor %}
  </ul>

