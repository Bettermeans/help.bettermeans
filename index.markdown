---
layout: default
title: Welcome
---

Welcome to the Bettermeans Help site. Here we have compiled many guides to help you set up.

Use the sidebar on the right side of this site to access our guides!


Getting started with Bettermeans
-----------------------------------

If you need help or have any questions, feel free to [contact support](mailto:support@bettermeans.com) or [ask the community](https://secure.bettermeans.com/projects/21/boards/39).

Popular guides
--------------

<dl>
  {% for post in site.categories.popular reversed %}
    <dt><a href="{{ post.url }}" id="{{ cat }}">{{ post.title }}</a></dt>
    <dd>{{ post.description }}</dd>
  {% endfor %}
</dl>

Getting help
------------

There are a number of resources available to help you

### Subtitle one

* [GitHub Status](http://status.github.com) -- Is GitHub down, or is it just me?

### Git

* [git ready](http://www.gitready.com/) -- Tips and hints for using git.
