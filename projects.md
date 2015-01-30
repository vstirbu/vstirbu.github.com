---
layout: page
title: Projects
permalink: /projects/
priority: 20
---

{% for publication in site.projects %}
{% if publication.href %}
#### [{{ publication.title }}]({{ publication.href }})
{% else %}
#### {{ publication.title }}
{% endif %}
{{ publication.content }}
{% endfor %}
