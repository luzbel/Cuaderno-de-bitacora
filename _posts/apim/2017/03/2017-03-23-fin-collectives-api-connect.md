---
title: "El fin de los *collectives* en API Connect"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
  - Microgateway
  - Collective
  - Contenedor
  - Docker
excerpt: "IBM anuncia evolución de la parte de ejecución de APIs en API Connect, deshaciéndose de los *collectives* y migrando a contenedores." 
header:
  teaser: /assets/images/apiconnect_logo.png
---

Sigue la avalancha de anuncios (veremos cuando los tendremos disponibles) sobre API Connect. Hoy repaso la [noticia](https://developer.ibm.com/apiconnect/2017/03/17/api-connect-enabled-containers/) de que, por fin, van a desaparecer los *collectives* y se va a evolucionar a algo mucho más estándar como los contenedores.

> API Connect is evolving its runtime management strategy from API Connect Collective Controllers towards running microservices in containers.
>
> \- [Meriç Aydonat](https://developer.ibm.com/apiconnect/2017/03/17/api-connect-enabled-containers/)

En primer lugar, no entiendo esa obsesión por parte de algunos fabricantes (IBM, Tibco, ...) de que el producto de gestión de APIs no solo gestione y también cubra el desarrollo del *backend* que da soporte al API. Tu gestiona mis APIs, y ya haré yo comoo quiera mis microservicios. En todo caso, dame un producto independiente para hacerlo.

En segundo lugar, nunca había entendido los *collectives*. No me entraba en la cabeza que para ejecutar un microservicio desarrollado en Node con Strongloop+Loopback necesitase nada que tuviese que ver con Java. Es decir, el tener que instalar los *collectives* siempre me había echado atrás y no había probado las capacidades de microservicios y microgateway de API Connect. Así que espero este cambio por parte de IBM y espero poder usarlo con OpenShift sin problemas, ya que la noticia solo hace referencia a Bluemix y parece dar a entender que se podrá usar con otras herramientas de orquestación, seguramente Kubernetes. 



