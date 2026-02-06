---
layout: default
title: Projects
permalink: /projects/
---

<div class="hero">
  <h1>Projects</h1>
  <p>개인/팀 프로젝트 정리</p>
</div>

<div class="grid">
  {% assign items = site.posts | where: "category", "project" | sort: "date" | reverse %}
  {% for post in items %}
    <div class="card">
      <!-- <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3> -->
      <h3>🚧공사중🚧</h3>
      <div class="meta">
        <span>{{ post.date | date: "%Y-%m-%d" }}</span>
        {% if post.tags %}<span>#{{ post.tags | join: " #" }}</span>{% endif %}
      </div>
      {% if post.excerpt %}
        <p style="color:var(--muted); margin:10px 0 0;">
          {{ post.excerpt | strip_html | truncate: 140 }}
        </p>
      {% endif %}
    </div>
  {% endfor %}
</div>
