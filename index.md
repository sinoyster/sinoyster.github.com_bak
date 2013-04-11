---
layout: page
title: Yuk Wong 的自言自语!
tagline: Supporting tagline
---
{% include JB/setup %}

## blogs list

  {% for post in site.posts %}
    <div class="posts">
      <p><h2><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h2>{{ post.date | date_to_string }} - {{site.name}}</p>
      <p>{{ post.content | strip_html | truncatewords: 55 }}</p>
      <p><a href="{{ BASE_PATH }}{{ post.url }}">Read more ...</a></p>
    </div>
  {% endfor %}

## To-Do

This theme is still unfinished. If you'd like to be added as a contributor, [please fork](http://github.com/plusjade/jekyll-bootstrap)!
We need to clean up the themes, make theme usage guides with theme-specific markup examples.


