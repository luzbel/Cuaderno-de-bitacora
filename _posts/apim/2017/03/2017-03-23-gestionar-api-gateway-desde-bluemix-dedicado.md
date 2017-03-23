---
title: "Gateways on-premise en Bluemix dedicado"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
  - Microgateway
  - OSS
excerpt: "IBM permite incluir gateways on-premise en instalaciones de API Connect sobre Bluemix dedicado"
header:
  teaser: /assets/images/apiconnect_logo.png
---

Sigo revisando anuncios de IBM en periodo [Interconnect](https://developer.ibm.com/apiconnect/2017/03/17/all-about-apis-at-interconnect-2017/):[facilitar la publicación como API][fastAPIs-post],[exportar analíticas][3partyanalytics-post],[liberación código fuente microgateway][liberacion-microgateway-post]

Hoy repaso la [noticia](https://developer.ibm.com/apiconnect/2017/03/19/manage-api-gateway-api-connect-bluemix-dedicated/) sobre la posibilidad de incluir y usar API Gateways desplegados on-premise sobre instalaciones de API Connect en Bluemix dedicado. Recordemos que un Bluemix dedicado es administrado por IBM y la infraestructura se encuentra en CPDs de la propia IBM. Puedo entender la necesidad que los gateways se encuentren próximos a los *backends* por motivos de rendimiento, pero:

* Estás pagando por una infraestructura, y resulta que tienes que poner parte y no una precisamente barata.
* Si el API va a estar expuesta a Internet, pues casi mejor que el gateway que esté expuesto a los ataques sea el de IBM y no el nuestro
* Si el API va a ser interno y no necesitamos las capacidades de seguridad de Datapower, ¿no es mejor desarrollarla para el microgateway? En ese caso, nuestra instalación va a ser más sencilla y quizás no tenga tanto sentido un Bluemix dedicado.
* Aunque el gateway lo pueda tener on-premise por rendimiento, seguirá siendo gestionado por IBM. Aunque hay distintas opciones para lidiar con el problema, hay que plantearse como gestionar el material criptográfico. Por ejemplo, si una política necesita de una clave privada de firma ¿querrás que los administradores de IBM tengan acceso a ella?

[fastAPIs-post]: {{ "" | absolute_url }}{% post_url serverless/2017/03/2017-03-20-api-connect-openwhisk-cloud-foundry %}
[3partyanalytics-post]: {{ "" | absolute_url }}{% post_url apim/2017/03/2017-03-18-Api-Connect-analiticas %}
[liberacion-microgateway-post]: {{ "" | absolute_url }}{% post_url apim/2017/03/2017-03-23-ibm-open-sources-microgateway %}