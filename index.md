---
layout: default
title: Home
---

<style>
.page-header {background-image: linear-gradient(120deg, #0aa2a9, #057d83);}
.project-name, .project-tagline, .main-content {font-family: Arial, Helvetica, sans-serif;}
.main-content p {line-height: 1.45;}
a {text-decoration: none;} a:hover {text-decoration: underline;}
</style>

# KT CyberMath • Statistics

Welcome. This site hosts my homework for the Statistics course.

## Homework feed
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <small> — {{ post.date | date: "%B %Y" }}</small>
    </li>
  {% endfor %}
</ul>
