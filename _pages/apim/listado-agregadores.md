---
title: "Listado de agregadores de APIs"
layout: single
permalink: /apim/listado-agregadores-apis.html
sidebar:
    nav: "altnav"
---

Para el concepto, esta [introducción](http://blogs.mulesoft.org/introducing-apihub/)(orientada a presentar su producto) es muy buena. En resumen, aún no hay el equivalente a Amazon (compras) o Facebook (social) para localizar APIs.

{% for item in site.data.apim.listado-agregadores %}
- [{{ item.nombre }}]({{ item.url }})
  - {{ item.descripcion }}
{% endfor %}
