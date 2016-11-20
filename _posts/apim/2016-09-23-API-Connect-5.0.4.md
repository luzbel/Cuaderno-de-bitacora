---
title: "Novedades IBM API Connect 5.0.4"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
excerpt: "Novedades API Connect 5.0.4"
---

Ya está disponible para descarga la versión 5.0.4 de IBM API Connect, en la que aparte de muchas correcciones de bugs, aparecen estas novedades:

* Advanced XML options
* Secure your APIs with IBM MobileFirst™ Foundation
* Ability to view and export API event data from Analytics
* Toolkit CLI accessibility mode
* New CLI commands ( apic orgs:get and apic devapps )
* Automatic subscription support with the Micro Gateway
* Ability to create an API and Product definition from a custom template using the API Designer
* Link checking in the Developer Portal
* Default language for code snippets in the Developer Portal

El detalle, como siempre, en la [web del producto](http://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.overview.doc/overview_whatsnew.html?cp=SSMNED_5.0.0)

De entre todas las novedades, resulta especialmente interesante la integración con MobileFirst.
Actualmente, el soporte OAuth en la mayoría de los gestores de APIs está limitado a algún "auth server" interno que trae el producto. Esto es así, debido a que la gestión de APIs suele basarse en el control de las aplicaciones clientes, y los "client_id" de OAuth deben ser los mismos. Teniendo el "auth server" interno, esto está solucionado. Pero ¿y si el auth server interno no cubre mis necesidades o ya tengo uno totalmente operativo? Por ejemplo, podría requerir un two-factor auth que ya tengo en mi auth server y que sería complejo y costoso de duplicar en el auth server interno del gestor de APIs.
En este aspecto, de todas las soluciones de gestión de APIs, WSO2 parece más avanzado y, por contra, la integración de IBM API Connect con "auth servers" externos era casi imposible. Esperamos que la integración con el auth server de MobileFirst sea un primer paso que abra las puertas a integraciones con otros auth servers y no depender del auth server interno que trae el producto.
