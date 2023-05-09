---
layout: default
title: Konsulenter
---

<h1>Konsulenter</h1>
<div class="konsulentliste">

{% for konsulent in site.konsulenter %}
<a href="{{ konsulent.url }}">{{ konsulent.navn }}</a>
{% endfor %}

</div>
