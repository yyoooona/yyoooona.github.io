---
title: "Post about Stack"
layout: archive
permalink: /Algorithm/Stack
author_profile: true
sidebar_main: true
---

{% assign posts = site.Algorithms.Stack | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
