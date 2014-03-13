---
layout: blog
title: Archive
---



<div style="margin-left:5em;">
	# Blog Posts
	<ul>
{% for post in site.posts %}
  	<li> {{ post.date | date_to_string }} &raquo; <a href="{{ post.url }}">{{ post.title }}</a> </li>
{% endfor %}
	</ul>
</div>