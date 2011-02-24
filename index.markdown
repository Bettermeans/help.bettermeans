---
layout: default
title: Welcome
---

Welcome to the Bettermeans Help site. Here we have compiled many guides to help you set up.

Use the sidebar on the right side of this site to access our guides!


Getting started with Bettermeans
-----------------------------------

As a first step, read the [getting started section](insertlink) and watch the tour

If you need help or have any questions, feel free to [contact support](mailto:support@bettermeans.com) or [ask the community](https://secure.bettermeans.com/projects/21/boards/39).

<iframe title="YouTube video player" width="640" height="390" src="http://www.youtube.com/embed/0wJAf229YUs" frameborder="0" allowfullscreen></iframe>

Popular guides
--------------

<dl>
  {% for post in site.categories.popular reversed %}
    <dt><a href="{{ post.url }}" id="{{ cat }}">{{ post.title }}</a></dt>
    <dd>{{ post.description }}</dd>
  {% endfor %}
</dl>


