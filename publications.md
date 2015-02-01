---
layout: page
title: Publications
permalink: /publications/
priority: 50
---

{% for publication in site.publications reversed %}
{% include publication.html %}
{% endfor %}
