---
title: "AWS API Gateway Developer Portal"
categories:
  - APIm
  - Serverless
tags:
  - APIm
  - AWS
  - Serverless
  - Monetizacion
  - ApiPortal
  - ApiGateway
excerpt: "Amazon mejora su oferta de gestión de APIs al incorporar un portal del desarrollador"
header:
  teaser: /assets/images/AmazonWebservices_Logo.svg
---

No consideraba al API Gateway de Amazon una solución completa de gestión de APIs, ya que cuando lo ví en el momento de su salida, se trataba de solo un gateway y parecía muy orientado al mundo AWS. 
Ahora la situación ha cambiado y recientemente Amazon ha liberado una [implementación de referencia](https://github.com/awslabs/aws-api-gateway-developer-portal) de su portal.

Justo cuando el otro día me preguntaba que [aplicaciones reales]({% post_url serverless/2016-11-29-ejemplo-app-chat-serverless %}) se podían construir ya con arquitectura serverles, me resulta bastante interesante que el portal se trate de una aplicación "serverless" construida sobre su, también recientemente liberada, librería [aws-serverless-express](https://github.com/awslabs/aws-serverless-express), la cual permite utilizar AWS Lambda y Amazon API Gateway para responder a peticiones web o APIs usando Node (O sea, construir aplicaciones, servicios webs y APIs RESTful usando un Express serverless).
Aparte de la curiosidad técnica y el probable ahorro de coste, volviendo a revisar la plataforma, lo que también hace más interesante a AWS como plataforma de gestión de APIs a considerar es:

* Se pueden crear las APIs a partir de Swagger
* Los planes de uso permiten establecer límites de consumo y empiezan a parecer un gobierno y gestión de APIs más elaborado
* Y lo más importante, y dónde veo que puede sacar ventaja a otros competidores, **Monetización**: El AWS API Gateway se integra con el AWS *Marketplace*, y no solo tu API aparece en el *market*, es que también puedes cobrar por su uso al tener acceso a toda la información de consumo. Con IBM API Connect y Bluemix, tu API puede aparecer en el *marketplace* de Bluemix, pero diría que el cobro es algo que tienes que gestionar tu directamente con tus consumidores. 