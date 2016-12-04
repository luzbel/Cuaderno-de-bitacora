---
permalink: /apim/listado.html
---

{% for producto in site.data.apim.listado-productos %}
- [{{ producto.nombre }}]({{ producto.url }})
{% endfor %}