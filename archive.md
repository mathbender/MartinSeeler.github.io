---
layout: blog
title: Archive
---

<div style="max-width:47em;margin-left:auto;margin-right:auto;">
	<h1> Blog Posts</h1>
	<ul>
	{% for post in site.posts %}
  		<li> {{ post.date | date_to_string }} &raquo; <a href="{{ post.url }}">{{ post.title }}</a> </li>
	{% endfor %}
	</ul>
</div>