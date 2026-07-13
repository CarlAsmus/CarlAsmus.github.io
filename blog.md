---
layout: page
title: Blog
permalink: /blog/
---

{% assign all_tags = "" | split: "" %}
{% for post in site.posts %}
  {% for tag in post.tags %}
    {% unless all_tags contains tag %}
      {% assign all_tags = all_tags | push: tag %}
    {% endunless %}
  {% endfor %}
{% endfor %}
{% assign all_tags = all_tags | sort %}

<div class="tag-filter-bar">
  <label for="tag-select">Filter by tag:</label>
  <select id="tag-select">
    <option value="all">All Posts</option>
    {% for tag in all_tags %}
      <option value="{{ tag }}">{{ tag }}</option>
    {% endfor %}
  </select>
</div>

<div class="post-list">
  {% for post in site.posts %}
  <div class="post-preview" data-tags="{{ post.tags | join: ',' }}">
    <div class="post-meta">{{ post.date | date: "%B %-d, %Y" }}</div>
    <h2 class="post-preview-title"><a href="{{ post.url }}">{{ post.title }}</a></h2>
    {% if post.tags.size > 0 %}
    <div class="post-tag-list">
      {% for tag in post.tags %}
        <span class="post-tag">{{ tag }}</span>
      {% endfor %}
    </div>
    {% endif %}
    {% if post.excerpt %}
    <p class="post-excerpt">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
    {% endif %}
    <a href="{{ post.url }}" class="read-more">Read more →</a>
  </div>
  {% endfor %}
</div>

<script>
  const filters = document.querySelectorAll('.tag-filter');
  const posts = document.querySelectorAll('.post-preview');

  const select = document.getElementById('tag-select');
  select.addEventListener('change', () => {
    const tag = select.value;
    posts.forEach(post => {
      const tags = post.dataset.tags.split(',');
      post.style.display = (tag === 'all' || tags.includes(tag)) ? '' : 'none';
    });
  });
</script>
