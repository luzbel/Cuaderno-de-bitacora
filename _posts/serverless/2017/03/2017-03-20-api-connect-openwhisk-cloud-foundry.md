---
title: "IBM API Connect+OpenWhisk+Cloud Foundry en Bluemix"
categories:
  - Serverless
tags:
  - Serverless
  - OpenWhisk
  - APIConnect
  - Bluemix
  - CloudFoundry
  - APIm
excerpt: "Como publicar en Bluemix de forma **sencilla, rápida y simple** APIs a partir de tus aplicaciones Cloud Foundry o tus acciones OpenWhisk"
header:
  teaser: /assets/images/apiconnect_logo.png
---


He tardado tanto en [dejar por escrito mis impresiones][lecciones-aprendidas-openwhisk-post] y lecciones aprendidas de escribir un bot para Telegram con OpenWhisk, que IBM ha tenido tiempo de reaccionar y [anunciar](https://developer.ibm.com/apiconnect/2017/03/17/coming-soon-30-seconds-serverless-traditional-managed-apis/) una importante mejora que cubriría una de mis principales quejas. 

Cuando llegue este funcionalidad (es lo que tienen los *"coming soon"*, que no sabes cuando), IBM se pondrá a la altura de Amazon que, como ya he comentado en otras ocasiones y a pesar de llevar su API Gateway menos tiempo en el mercado, destaca por la integración de su gateway con la plataforma (hablo sin ser usuario de AWS y solo en base a documentación). Con esta funcionalidad se podrá publicar en Bluemix de forma **sencilla, rápida y simple** APIs a partir de tus aplicaciones Cloud Foundry o tus acciones OpenWhisk. No se puede sacar mucha información del par de vídeos publicados por IBM, pero me gustaría saber:

* ¿tendrás que tener una instancia de API Connect o se usará una interna creada de forma transparente?
* Cada vez que se invoque a un API, ¿se contabilizará como consumo en la instancia/servicio de API Connect o estará incluido en lo que pago por mi aplicación CloudFoundry o mi *action" OpenWhisk? La lógica me dice que se cobrará aparte la aplicación del número de invocaciones al API, pero como IBM no desglosa tanto en otros servicios en su nube (por ejemplo, no suelen cobrar aparte el tráfico de red), pues todo es posible. Veremos.
* ¿podrás engancharlo con un API Connect distinto del público, como uno desplegado en un Bluemix dedicado, en un Bluemix local, o simplemente en un *on-premise* en mi CPD?
* ¿podré acceder a las definiciones de las APIs para poder modificar su ensamblado y añadir políticas (logs, seguridad extra, etc.)?
* ¿estas APIs correran en Datapower o en el microgateway?
* Google anunciaba hace tiempo la posibilidad en su nube de que el API Gateway no fuese centralizado y estuviese más pegado a la aplicación ¿cómo se instanciaran estas APIs en Bluemix? ¿podrían correr en un microgateway distinto para cada aplicación y no en un Datapower centralizado? 
* ¿desaparece con esta nueva funcionalidad la [beta](https://www.ibm.com/blogs/bluemix/2017/01/exposing-openwhisk-restful-apis-api-gateway/) anterior o son funcionalidades diferentes?  
* La capacidad de *social login* (en el vídeo aparecen Google, Facebook e IBM) es muy interesante. Se supone que en el ecosistema Bluemix MobileFirst lo cubre, pero siempre me ha parecido complejo y demasiada parafernalia para proyectos sencillos. La pregunta es, las autenticaciones y autorizaciones las hará el proveedor que selecciones y me cobrará por ello, pero ¿me cobrará IBM alguna tarifa por la gestión o entrará dentro del coste de las llamadas a APIs o del propio servicio (CF o OpenWhisk)? 

[lecciones-aprendidas-openwhisk-post]: {{ "" | absolute_url }}{% post_url serverless/2017/03/2017-03-20-lecciones-bots %}