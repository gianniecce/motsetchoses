---
layout: page
title: archive
permalink: /archive/
---

  <ul class="post-list">
    {% for post in site.posts %}
      <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
      <p>
      <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }} <!-- post.content | strip_html | truncatewords: 5 -->
      </a>
      </p>
      </li>
    {% endfor %}
  </ul>
