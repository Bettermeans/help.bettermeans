---
layout: default
title: Welcome
---

Welcome to the Bettermeans Help site. Here we have compiled many guides to help you set up.

Use the sidebar on the right side of this site to access our guides!

Getting started with Bettermeans
-----------------------------------

As a first step, read the [getting started section](/quickstart) and watch the tour

If you need help or have any questions, feel free to [ask the community](https://secure.bettermeans.com/projects/21/boards/39) or [contact support](mailto:support@bettermeans.com).

<object width="640" height="390">
  <param name="movie" value="http://www.youtube.com/v/0wJAf229YUs"></param>
  <param name="allowFullScreen" value="true"></param>
  <embed src="http://www.youtube.com/v/0wJAf229YUs"
  type="application/x-shockwave-flash" allowfullscreen="true"
  width="640" height="390"></embed>
</object>

Popular guides
--------------

<dl>
  {% for post in site.categories.popular reversed %}
    <dt><a href="{{ post.url }}" id="{{ cat }}">{{ post.title }}</a></dt>
    <dd>{{ post.description }}</dd>
  {% endfor %}
</dl>


