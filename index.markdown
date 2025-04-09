---
layout: default
title: Your Blog Name
---

<div class="intro">
  <p>Hey there! 👋 Welcome to my minimalist dark-themed blog.</p>
  <p>Here you'll find my latest thoughts, tutorials, and personal experiments. Scroll down to explore!</p>
</div>

<hr style="margin: 2rem 0; border: none; border-top: 1px solid #333;" />

<div class="latest-posts">
  <h2>Latest Posts</h2>
  <div class="post-list">
    {% for post in site.posts %}
      <div class="post-box">
        <a href="{{ post.url }}">{{ post.title }}</a>
        <div class="post-date">{{ post.date | date: "%B %d, %Y" }}</div>
      </div>
    {% endfor %}
  </div>
</div>
