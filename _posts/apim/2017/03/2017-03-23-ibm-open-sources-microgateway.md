---
title: "IBM libera el código de su microgateway"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
  - Microgateway
  - OSS
excerpt: "IBM libera con licencia Apache el código fuente de su microgateway"
header:
  teaser: /assets/images/apiconnect_logo.png
---

Coincidiendo con el [Interconnect](https://developer.ibm.com/apiconnect/2017/03/17/all-about-apis-at-interconnect-2017/) IBM está anunciado muchas novedades relacionadas con API Connect. 
A los recientes anuncios de [facilitar la publicación como API][fastAPIs-post]  de acciones de OpenWhisk y servicios CloudFoundry y de [exportar analíticas a sistemas externos de terceros][3partyanalytics-post], hoy me doy cuenta del anuncio de la liberación con licencia Apache del código fuente de su microgateway ([recordemos][apiconnect5-post] que [compraron StrongLoop][microgateway-post]).

Del primer vistazo del [repositorio](https://github.com/strongloop/microgateway) en GitHub veo:

* Parece que el microgateway se puede usar por si solo, sin necesidad de estar gestionado por el API Manager. Puede ser interesante para facilitar las pruebas para los desarrollos. Por contra, el dominio sobre el que corre el gateway en Datapower solo puede ser generado por el API Manager.
  * Si esto es así, aún entiendo menos que pinta los [*collectives*](https://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.install.doc/tapic_install_collective.html) y porque es un [prerequisito](https://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.install.doc/tapic_install_microgw.html) tenerlos para usar los microgateways en una instalación de API Connect.
* Políticas
  * Veo que incluso las más básicas que vienen de caja están [implementadas](https://github.com/strongloop/microgateway/tree/master/policies) en Javascript
  * Curioso el método de [*throttling*](https://github.com/strongloop/microgateway/tree/master/policies/rate-limiting) usando Redis
  * Cada vez tengo más claro que debes pensar antes de implementar un API tienes que tener muy claro si lo vas a hacer para un gateway sobre Datapower o para el microgateway, ya que son incompatibles a nivel de ensamblado
* Parece que hay predisposición por parte de IBM para aceptar [contribuciones](https://github.com/strongloop/microgateway/blob/master/CONTRIBUTING.md) 
* En cierto modo, me recuerda bastante a [Kong](https://getkong.org/): Basado en nginx, liberado bajo OSS, asociado a un API Manager y API Portal de pago, pero con la diferencia de que Kong es extensible mediante LUA y este por Javascript.
  * Creo recordar que hace varias versiones traté de ejecutar Kong en OpenShift y no se podía al requerir el contenedor privilegios de administrador. ¿funcionará el microgateway de IBM en OpenShift o solo en Docker puro? 
* ¿qué rendimiento dará?
* Si lo consideras para APIs públicas y expuestas a entornos hostiles en lugar del gateway basado en Datapower de API Connect, sería necesario algún elemento de seguridad por delante (que podría ser un Datapower, F5 o similar), no para temas relativos al API (*throttling*, OAuth, etc.), pero sí para protección contra ataques DOS, inyección SQL, etc.

A todo esto, Redhat [compró 3scale][3scale-redhat-post] ya hace tiempo. Su primer movimiento tras comprar suele ser liberar el código fuente. Que yo sepa aún no lo han hecho y resulta curioso que el dinosaurio IBM lo haga antes con un producto también comprado.

[fastAPIs-post]: {{ "" | absolute_url }}{% post_url serverless/2017/03/2017-03-20-api-connect-openwhisk-cloud-foundry %}
[3partyanalytics-post]: {{ "" | absolute_url }}{% post_url apim/2017/03/2017-03-18-Api-Connect-analiticas %}
[apiconnect5-post]: {{ "" | absolute_url }}{% post_url apim/2016-03-28-novedades-api-connect-5 %}
[microgateway-post]: {{ "" | absolute_url }}{% post_url apim/2016-11-22-gateways-microgateways %}
[3scale-redhat-post]: {{ "" | absolute_url }}{% post_url apim/2016-06-23-redhat-compra-3scale %}