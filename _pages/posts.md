---
layout: page
title: Posts
permalink: /posts
image: "assets/images/work.jpg"
---

[![github-sponsor-button](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)](https://github.com/sponsors/devantler)

A collection of my blog posts on software development, Kubernetes, DevOps, and more.

---

{% for post in site.posts %}

## [{{ post.title }}]({{ post.url | relative_url }})

<span class="post-date">{{ post.date | date: "%B %-d, %Y" }}</span>

{{ post.excerpt | strip_html | truncatewords: 50 }}

[Read more →]({{ post.url | relative_url }})

---

{% endfor %}
