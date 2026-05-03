---
layout: default
title: "Son Yazılar 📝"
permalink: /son-yazilar/
---

# Son Paylaşımlar

Burada yazılım, otomasyon ve karavan hayatına dair en güncel içeriklerimi bulabilirsin.

<ul class="post-list">
  {% for post in site.posts %}
    <li style="margin-bottom: 25px; list-style: none;">
      <span class="post-meta" style="color: #666; font-size: 0.9rem;">{{ post.date | date: "%-d %b %Y" }}</span>
      <h3>
        <a class="post-link" href="{{ post.url | relative_url }}" style="font-size: 1.5rem !important;">
          {{ post.title | escape }}
        </a>
      </h3>
      <p style="margin-top: 5px; font-size: 0.95rem; color: #444;">
        {{ post.excerpt | strip_html | truncatewords: 30 }}
      </p>
    </li>
  {% endfor %}
</ul>
