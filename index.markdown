---
layout: default
title: snipExample
---

<div class="intro">
  <p>Hey there! 👋 Welcome to snipExample.</p>
  <p>Concepts simplified with application based examples!</p>
</div>

<hr style="margin: 2rem 0; border: none; border-top: 1px solid #333;" />

<div class="latest-posts">
  <h2>Latest Blogs</h2>
  <div class="post-list">
    {% for post in site.posts %}
      <div class="post-box">
        <a href="{{ post.url }}">{{ post.title }}</a>
        <div class="post-date">{{ post.date | date: "%B %d, %Y" }}</div>
      </div>
    {% endfor %}
  </div>
</div>
