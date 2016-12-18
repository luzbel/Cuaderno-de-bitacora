---
title: "Listado plataformas serverless"
layout: single
permalink: /serverless/listado-plataformas-serverless.html
---

{% for producto in site.data.serverless.listado-plataformas%}
- [{{ producto.nombre }}]({{ producto.url }})
{% endfor %}