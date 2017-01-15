---
layout: archive
permalink: /cine/
title: "Películas vistas por año y categoría"
---
{% include toc %}

Las películas que he visto desde 2016

{% assign filmsByYear = site.data.cine.listado-peliculas | group_by:"anyo" %}

# Por año
{% for anyo in filmsByYear %}
## {{ anyo.name}}
 {% for item in anyo.items %}
{:.no_toc}
### {{ item.nombre }}
Plataforma: {{ item.plataforma }}
 {% endfor %}
{% endfor %}

{% assign filmsByPlatform = site.data.cine.listado-peliculas | group_by:"plataforma" %}

# Por plataforma
{% for plataforma in filmsByPlatform %}
## {{ site.data.cine.listado-plataformas[plataforma.name].descripcion }}
 {% for item in plataforma.items %}
{:.no_toc}
### {{ item.nombre }}
Año: {{ item.anyo }}
 {% endfor %}
{% endfor %}
