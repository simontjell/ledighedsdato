---
layout: default
title: Konsulenter
---

<h1>Konsulenter</h1>

{% for konsulent in site.konsulenter %}
<a href="{{ konsulent.link }}">{{ konsulent.navn }}</a><br>
{% endfor %}
