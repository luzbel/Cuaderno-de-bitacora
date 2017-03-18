---
title: "Extraer analíticas en API Connect 5.0.7"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
  - Analíticas
  - Kafka
  - Splunk
excerpt: "Conectar los eventos de API Connect a Kafka y otros sistemas externos"
header:
  teaser: /assets/images/apiconnect_logo.png
---

Ya he comentado en alguna ocasión que el diseño monolítico de API Connect tiene impacto en su rendimiento y estabilidad. La sensación es que tener en el *API Manager* todo un sistema de analíticas con Logstash y ElasticSearch provoca un alto consumo de RAM. Parece que es algo que se resolverá en la versión [Docker](https://hub.docker.com/r/ibmcom/apiconnect/) donde [vemos](https://github.com/strongloop/apiconnect-docker/blob/master/docker-compose.yaml) que Logstash y ElasticSearch ya se han sacado del API Manager facilitando su escalado por separado.

También es cierto que cuando compras un producto de un fabricante esperas que cubra todos tus necesidades incluyendo las analíticas, pero siempre es bueno que te facilite la integración con otros sistemas. Por ejemplo, igual quiero hacer procesamiento en tiempo real de las invocaciones a mis APIs para detectar patrones que puedan disparar alarmas de seguridad. O si tengo ya mi sistema de monitorización que cubre mis otras aplicaciones y sistemas, ¿por qué tengo que navegar a la CMC para ver el estado de mis nodos de API Connect?

Así que la nueva funcionalidad que anuncia este [artículo](https://developer.ibm.com/apiconnect/2017/03/16/real-time-api-analytics-splunk-sap-others/) para la próxima versión 5.0.7 va a ser realmente útil y apreciada.

Las salidas soportadas inicialmente (HTTP, ElasticSearch, syslog y Kafka) me parecen bastante acertadas ya que cubrirán la mayoría de los casos reales de uso.

En cuanto a las opciones de configuración, aparece la posibilidad de que las analíticas se pasen únicamente al sistema externo y no sean accesibles desde la interfaz de usuario del API Manager. Supongo que no almacenar las analíticas en el API Manager aligerará su carga mejorando su estabilidad y rendimiento, pero la pregunta que me hago y que será lo primero que pruebe cuando se libere la versión 5.0.7 es si las analíticas de los eventos de APIs se generarán directamente desde los gateways o si seguirán pasando siempre por el API Manager. En el segundo caso, aunque ganemos en que los datos se enriquezcan (con detalles que los gateways no necesitan como los nombres de las aplicaciones), quizás la actualización ya no sea tan en tiempo real (como vende el artículo), ya que los eventos se tendrán que consolidar en el API Manager antes de llegar a nuestro sistema externo de recopilación de analíticas. ¿impedirá esto el RTP sobre las invocaciones a las APIs?
. También, como siempre, me preocupa si todo esto va a ser compatible solo con Datapower o si también valdrá para el microgateway.

Por último, y también como siempre, el API Portal sigue siendo el componente al que menos caso se hace, y hecho en falta eventos de navegación en el portal que pueden ser muy útiles para conocer a nuestros clientes (los consumidores de APIs y desarrolladores de aplicaciones). Por ejemplo, ¿por qué un API atrae a bastantes desarrolladores que prueban el *sandbox* pero luego no se materializa en suscripciones al API real? ¿por qué se descarga la documentación de un API pero luego no se prueba? ¿se visita el foro pero se abandona la navegación porque no se encuentran entradas?



