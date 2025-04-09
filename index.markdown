---
layout: default
title: Your Blog Name
---

<div class="intro">
  <p>Hey there! 👋 Welcome to my minimalist dark-themed blog.</p>
  <p>Here you'll find my latest thoughts, tutorials, and personal experiments. Scroll down to explore!</p>
</div>

<hr style="margin: 2rem 0; border: none; border-top: 1px solid #333;" />

<h2>Latest Posts</h2>

<ul style="list-style: none; padding: 0;">
  {% for post in site.posts %}
    <li style="margin: 1rem 0; text-align: center;">
      <a href="{{ post.url }}" style="color: #a9c9ff; font-size: 1.2rem;">{{ post.title }}</a>
      <div style="font-size: 0.9rem; color: #888;">{{ post.date | date: "%B %d, %Y" }}</div>
    </li>
  {% endfor %}
</ul>
