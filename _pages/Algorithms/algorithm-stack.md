---
title: "Post about Stack"
layout: archive
permalink: /algorithms/stack
author_profile: true
sidebar_main: true
---

{% assign posts = site.algorithms.Stack | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
