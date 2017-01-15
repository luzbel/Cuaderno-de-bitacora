---
layout: archive
permalink: /rol/
title: "Partidas de rol y juegos de mesa por año y juego"
---
{% include toc %}

Partidas de rol y juegos de mesa desde 2017

{% assign sessionsByGame = site.data.rol.listado-sesiones | group_by:"juego" %}
# Por juego
{% for juego in sessionsByGame %}
## {{ site.data.rol.listado-juegos[juego.name].descripcion }}
Número de sesiones: {{ juego.items | size }}
{% endfor %}

{% assign sessionsByYear = site.data.rol.listado-sesiones | group_by:"anyo" %}
# Por año
{% for anyo in sessionsByYear %}
## {{ anyo.name}}
Número de sesiones: {{ anyo.items | size }}
{% endfor %}