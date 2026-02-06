---
layout: default
title: Logs
permalink: /logs/
---

<div class="hero">
  <h1>Logs</h1>
  <p>전체 학습 기록 모음</p>
</div>

<div class="grid">
  {% for post in site.posts %}
    <div class="card">
      <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
      <div class="meta">
        <span>{{ post.date | date: "%Y-%m-%d" }}</span>
        {% if post.tags %}<span>#{{ post.tags | join: " #" }}</span>{% endif %}
      </div>
    </div>
  {% endfor %}
</div>
