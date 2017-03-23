---
title: "Monetizacion en API Connect"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
  - Monetización
excerpt: "Monetización en API Connect"
header:
  teaser: /assets/images/apiconnect_logo.png
---

Otra noticia reciente sobre API Connect, esta vez [anunciando](https://developer.ibm.com/apiconnect/2017/03/17/coming-soon-api-monetization-capability-ibm-api-connect/) capacidades de monetización en el producto.

De nuevo, no entiendo esta obsesión de los fabricantes de añadir todo tipo de funcionalidades a un producto de gestión de APIs, que lo que debería hacer es , eso, gestionar APIs (La verdad es que la monetización, sin llegar a comprender todo lo que supone, es algo que todos los clientes piden).

Pero desde mi punto de vista la monetización no debe tratarse en el gestor de APIs. No digo que no haya que monetizar y , de hecho, la monetización es un punto básico en una estrategia de APIs, pero no creo que se deba delegar en un producto de gestión de APIs. El producto de gestión de APIs se debe limitar a generar los CDRs (*call detail  records*) con toda la información que permita facturar, incluyendo las llamadas a las APIs por cliente y cualquier otra información relevante. Por supuesto, será necesario tratar al API como un producto y no como una mera interfaz técnica (algunos productos de gestión de APIs parecen centrados sólo en resolver la parte técnica de integración y no facilitan la gestión como producto, la definición de planes de consumo, el control de las suscripciones por cliente, etc.): Huye de estos últimos gestores de APIs.

Si tuviera que justificar esta visión, te daría 2 argumentos: complejidad y visión global


### Complejidad

Como cualquier facturación, la monetización es compleja. Ningún producto de gestión de APIs tendrá la capacidad para facturar adecuadamente. Incluir una página en el API Portal que multiplique el número de llamadas a un API por un precio por llamada, no es monetizar, y el fabricante que te venda eso como monetización te está engañando.

### Visión global

Detrás de un API podemos simplificar pensando que solo hay un *endpoint*, pero en realidad puede existir todo un ecosistema complejo de microservicios, sistemas *legacy*, sistemas intermedios de orquestación/integración. ¿de verdad vas a monetizar y calcular el coste sólo en base al número de llamadas del API?
Todos los sistemas involucrados en dar servicio deberían generar información que deberías recopilar para poder monetizar (y monitorizar) adecuadamente. No se trata solo de cobrar por el uso, también es necesario que conozcas el coste de cada llamada y por los sistemas que pasa y las operaciones que realiza para que puedas calcular el coste que te supone cada invocación a un API y establecer un precio adecuado.

## ¿qué pasará? 

Veremos que presenta finalmente IBM, pero la noticia da a entender que se ofrecerá tanto integración con sistemas de facturación externos (:+1:) como un sistema de facturación de caja (:thumbsdown:)

> ... having the flexibility to use the OOTB billing system or integrate with the organization’s existing enterprise billing system... 
>
> \- [SayeedMahmood](https://developer.ibm.com/apiconnect/2017/03/17/coming-soon-api-monetization-capability-ibm-api-connect/)


