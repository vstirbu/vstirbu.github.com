---
layout: page
title: Talks
permalink: /talks/
priority: 50
---

{% for talk in site.talks reversed %}
{% include talk.html %}
{% endfor %}