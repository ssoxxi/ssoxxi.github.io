---
layout: default
title: Home
---

<div class="hero">
  <h1>학습 기록</h1>
  <p>매일 쌓는 SQL · Web Dev · 문제해결 로그</p>
</div>

<div class="grid">
  {% for post in site.posts limit:6 %}
    <div class="card">
      <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
      <div class="meta">
        <span>{{ post.date | date: "%Y-%m-%d" }}</span>
        {% if post.tags %}<span>#{{ post.tags | join: " #" }}</span>{% endif %}
      </div>
      {% if post.excerpt %}<p style="color:var(--muted); margin:10px 0 0;">{{ post.excerpt | strip_html | truncate: 120 }}</p>{% endif %}
    </div>
  {% endfor %}
</div>
