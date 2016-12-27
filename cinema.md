---
layout: page
title: cinema
permalink: /cinema/
---



  <ul class="post-list">
    {% for post in site.tags.Cinema %}
      <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
      <p>
      <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}
      </a>
      </p>
      </li>
    {% endfor %}
  </ul>
