---
title: "Agregadores de APIs"
categories:
  - APIm
tags:
  - APIm
  - Agregadores
  - Hubs
  - APIHub
  - ProgrammableWeb
  - PublicAPIs
  - APIs.io
  - APIs.json
  - APIConnect
  - Bluemix
  - AWS
  - AmazonAPIGateway
  - AWSMarketPlace
  - Monetizacion
excerpt: "Listado de agregadores de APIs y algunos pensamientos sobre su futuro"
---

Mantenía un listado de agregadores en el grupo de gestión de APIs del yammer de mi empresa, pero he decidido moverlo a Jekyll al poder modificarlo de manera más fácil editando solo un YAML.
Puedes consultar el listado actualizado en este [enlace](/apim/listado-agregadores-apis).

El caso es que no hay a día de hoy un agregador de referencia, y todo el que hace API públicas tiene como uno de los pilares de su estrategia el publicar en su propio portal. Tener tu propio portal tiene muchas ventajas (algún día hablaremos de ellas) para el vendedor de APIs, pero desde el punto de vista del consumidor de APIs creo que la situación tiene que cambiar algún día. Hoy puede ser asumible tener que ir dándose de alta en cada uno de los portales de las APIs que consume nuestra aplicación, pero conforme vayamos consumiendo más APIs de distintos proveedores, esto puede llegar a ser inmanejable. Y no sólo por la gestión de usuarios en cada plataforma y client_id/client_secret (que seguro que querremos gestionar de una forma automatizada y el tenerlos en distintas plataformas no ayudará), también por la facturación al tener que lidiar de nuevo con varios proveedores.
Lo que quiero decir es que los agregadores tienen que dejar de ser un mero listado de APIs públicas y pasar a ser mercados completos donde suscribirse y pagar en un único punto APIs de diferentes proveedores, del mismo modo que pasa con las tiendas de aplicaciones de iOS, Android o Microsoft. 

En esta línea veo algunos movimientos incompletos:

- En Bluemix antes se podía añadir un API gestionada en "API Connect" en el catálogo de Bluemix, pero parece que ya no se puede. Aunque todavía existiese esa posibilidad, hay varios puntos a considerar para que esto sea útil:
  - El cobro debería realizarse contra la cuenta de Bluemix, y que este hiciese de intermediario con el proveedor del API.
  - Se debería gestionar el alcance de la publicación. Si, por ejemplo, estoy en un bluemix dedicado, debería poder decidir si mis APIs son privadas y sólo deben aparecer en el catálogo de mi bluemix dedicado, o si son públicas y también las quiero añadir al catálogo del Bluemix público.
    - Y, por supuesto, debería integrarse con un API Connect desplegado *on-premise*. 
  - En cualquier caso, estaría limitado a Bluemix, y tengo que considerar que no todos mis consumidores de APIs trabajan exclusivamente en esta plataforma.
- Aunque limitado al ecosistema de Amazon, el Amazon API Gateway se [integra](https://aws.amazon.com/about-aws/whats-new/2016/12/amazon-api-gateway-integration-with-aws-marketplace/) con el [*AWS MarketPlace*](https://aws.amazon.com/marketplace/saas), donde puedo incluir mis APIs y cobrar por su uso.