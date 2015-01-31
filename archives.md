---
layout: default
title: Archives
---

<h1>Archives</h1>
<div id="archives">
  {% for post in site.posts %}
    <h3>
      <time datetime="{{ post.date | date_to_xmlschema }}" class="pretty-date">{{ post.date | date_to_string }}</time> {{ post.title }}
    </h3>
    <i>{{ post.excerpt | remove: '<p>' | remove: '</p>' }}</i>
    <a href="{{ post.url }}" class="anchor"><small>...Read more.</small></a><br/><br/>
  {% endfor %}
</div>
