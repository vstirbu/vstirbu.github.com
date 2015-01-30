---
layout: page
title: Publications
permalink: /publications/
priority: 50
---

{% for publication in site.publications %}
{{ publication.title }}
{% endfor %}
