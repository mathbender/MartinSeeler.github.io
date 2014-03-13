---
layout: blog
title: Archive
---

# Blog Posts

<div style="margin-left:auto; margin-right:auto;">
{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}
</div>