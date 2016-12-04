---
title: "Créditos"
excerpt: "Recopilación de elementos usados en la web y sus respectivas licencias"
sitemap: false
permalink: /acerca-de/creditos.html
layout: single
author_profile: false
---

{% for credito in site.data.creditos %}
#### {{ credito.nombre }}
 {% if credito.atribucion %}
{{ credito.atribucion }}
 {% else %}
[{{ credito.descripcion}}]({{ credito.url }})
 {% endif %}
{% endfor %}