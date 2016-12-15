---
title: "OpenWhisk GA"
categories:
  - Serverless
tags:
  - Serverless
  - OpenWhisk
  
excerpt: "OpenWhisk deja de ser BETA y pasa a estar disponible de forma general"
header:
  teaser: /assets/images/Alfredo_Landa.teaser.jpg
---

Pues eso, que OpenWhisk deja de ser BETA. Os dejo por aquí el enlace al [anuncio](https://www.ibm.com/blogs/bluemix/2016/12/general-availability-openwhisk/) y a las instrucciones para [migrar](https://www.ibm.com/blogs/bluemix/2016/12/openwhisk-beta-general-availability-ga/) desde la BETA. 

Tengo que darle un vistazo más profundo, pero no veo nuevas funcionalidades significativas y me falta una mejor integración con la gestión de APIs (ya sea la de IBM u otra) y con OAuth. Para el backend usar "API Keys" puede ser valido, pero si precisamente queremos disminuir el uso de servidor, debería poder controlar que no se llama a mi acción Whisk de forma indiscriminada y descontrolada desde el frontal.

Si, supongo que puedes montar IBM MobileFirst+IBM API Connect+IBM OpenWhisk, pero me da la sensación que es un montaje más complejo del equivalente en el ecosistema AWS.