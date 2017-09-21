---
title: "Novedades API Connect 5.0.8"
modified: 2017-09-21
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
excerpt: "Repaso a la lista de novedades de IBM API Connect 5.0.8"
header:
  teaser: /assets/images/apiconnect_logo.png
---

Como siempre, en este [enlace](https://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.overview.doc/overview_whatsnew.html) puedes ver la lista completa de novedades del recién liberado IBM API Connect 5.0.8. No he podido actualizar y probarlas aún pero, como siempre, escribo mis primeras impresiones y las dudas de temas que quiero comprobar

Ojo, que IBM está siguiendo la fea costumbre de añadir funcionalidades incluso en las revisiones menores (ya lo hizo con la 5.0.7.1 y 5.0.7.2), por lo que igual cuando visitáis el enlace oficial de IBM os encontráis otras funcionalidades no comentadas. {:.notice}

## Added support and a reference for Developer Portal REST APIs for analytics

> Developer Portal REST APIs help you analyze your catalog APIs. For more information, see Analytics.

Vale, bien, nuevas analíticas. Ahora solo falta que funcionen porque hasta ahora esta era una zona plagada de errores, sobre todo en cuanto a la aplicación de filtros en las consultas.

También me resulta curioso que se llamen "Developer Portal REST APIs", cuando se invoca al Manager y no al Portal para obtenerlas.

## Added the Analytics section when creating an API

> You can define and specify existing Parameters for your API that can be used to gather analytics data about the API. See Composing a REST API definition for more information.

Este cambio no aparece aún en la versión desplegada en Bluemix y no tengo claro que permite. Parece que se pueden añadir campos a medida que se pueden buscar luego en las analíticas

## Added the logs option to the system clean command

> Specifying the logs option with the system clean command removes all of your log data from your server. For more information, see System commands.

Pues esto puede parecer una tontería, pero era básico y no ha llegado hasta la versión 5.0.8. Os sonará increible, pero cada vez que tenías un problema, el soporte de IBM te pedía que generases volcado de los logs y estos se iban acumulando y los únicos que podían borrarlos era IBM. No todo iban a ser ventajas en las *appliances*.

## Added the analytics option to the system clean command

> Specifying the analytics option with the system clean command removes all of your analytics data from your server. For more information, see System commands.

Igual que en el caso anterior, esto es algo básico para el mantenimiento del sistema y hasta ahora dependías de organizar una sesión remota con IBM para que entrasen en la *appliance* y borrasen esta información a mano y así liberar espacio.

Quizás hubiese sido interesante poder purgar lo anterior a una fecha sin necesidad de borrar todo. En cualquier caso, lo recomendable es que ya estés usando las capacidades desde la versión 5.0.7 de exportar las analíticas a tu lago de datos externo, por lo que podrías borrar todas las analíticas del *API Manager* periodicamente. 

## Encourage the use of two-factor authentication in the Developer Portal

> You can encourage users of your Developer Portal to set up two-factor authentication (TFA) on their account by applying a TFA Rules module. For more information, see Encouraging users to set up two-factor authentication on their Developer Portal account.

Una mejora más para el portal, poniendo la decisión cada vez más difícil a los que se plantean montar su portal y no usar el de caja.

## Features added to the integrated billing and payment management

> Administrator:
> Create monthly prepaid billing subscription Plans that your API customers can subscribe to with a credit card. See Billing for the use of your Products for more information.
>
> Leverage a Stripe account to manage the payments for your subscriptions.
>
> Specify a number of free trial days in your subscription Plan for new subscribers. Payment automatically begins after the trial days expire.
>
>Customer:
> Subscribe to fee-based Plans in the Developer Portal that allow you to use Products that contain one or more APIs. See Tutorial: Subscribing to a Plan with pricing for more information.

Parece que empieza a funcionar la monetización con [Stripe](https://stripe.com). No todo el mundo querrá trabajar con Stripe, pero es un comienzo.

# Invoke automatically replaced in the gateway

> The last invoke in your policy might be replaced by a proxy. This is done automatically by the gateway to improve performance. For more information, see: API properties.

Uhm, no tengo muy claro que cambiar el comportamiento de lo que ha definido el desarrollador sea buena idea. De todos modos, en mis reglas de QA particulares, siempre obligábamos a tener un *proxy* al final del ensamblado y nunca he estado muy de acuerdo con el uso de *invoke*, ya que terminas haciendo composición de servicios/orquestación y nunca me ha gustado en el API Gateway. Por lo menos en el clásico centralizado (Datapower o el equivalente en otras soluciones) ya que normalmente se ubicará en DMZ y no querré tener lógica de negocio ejecutándose allí. Con microgateways descentralizados, y más pegados a los servicios, y una capa de seguridad en DMZ por encima de ellos, sería planteable.

En cualquier caso, al examinar el [enlace](https://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.toolkit.doc/configuration_props.html) al que remite este punto de la documentación, me empieza a no gustar ese listado de propiedades que afectan a las políticas, en muchos casos dependiendo de la versión. Parece como si IBM se llevase la mala práctica de programar con *#define* al motor del gateway.

## The Linux distribution for the Developer Portal OVA is now based on Ubuntu Version 16.04

> Support for Debian Version 7 is coming to an end in May 2018, so the Linux distribution for the Developer Portal OVA is now based on Ubuntu Version 16.04. For information about how to migrate your current Debian OVAs to the Ubuntu OVAs, see Migrating your Developer Portal OVAs from Debian V7 to Ubuntu V16.04.

Vale que vaya a quedarse sin soporte Debian, pero ¿no sería mejor pasar a una nueva versión de Debian?.  ¿por qué Ubuntu? ¿por qué no proporcionar versiones homologadas de la *appliance* del portal en algunos de los sistemas Linux más usados a nivel empresarial (por ejemplo, Red Hat)?. En ocasiones parece que el producto se hace pensando en el equipo de IBM que lo mantiene en Bluemix, y no en los clientes que quieren su propia instalación en local.

¿qué pasa ahora con las empresas que han tenido que aprender a manejarse en Debian e integrar esta appliance en sus procesos de mantenimiento? ¿tiran todo ese trabajo y lo vuelven a adaptar para Ubuntu? ¿y cuando en la versión X.x.x IBM decidad volver a cambiar de sistema base y elegir, que se yo, SUSE)? Es que puedo entender las *appliances* para sistemas cerrados como Datapower o el *API Manager* (y eso que no deja de ser un linux), pero para ponerme un Debian o un Ubuntu con un Drupal no hace falta una *appliance* cerrada: Hace falta un contenedor Docker valido para producción del portal ¡¡¡ YA !!! 

Si es que encima el proceso no es transparente y requiere de un proceso de migrado. ¿para eso quiero una *appliance*?

## New API event fields

> Added the following API event fields:
> billing.trial_period_days
> billing.amount
> billing.currency
> billing.model
> billing.provider
> client_id
> immediate_client_ip
> latency_info2.task
> latency_info2.ended
>
> See API event record fields and Obtaining analytics data by using REST API calls for more information.

Obviamente se han añadido los nuevos campos de la monetización. Y veo un par más relativos a la latencia que pueden ser interesantes para pruebas de rendimiento (a ver que pasa finalmente con la nueva versión del [*API Gateway*](https://github.com/ibm-apiconnect/apigateway-experimental)).

## New query parameters for the Redirect URL

> New query parameters have been added to the information available for a third party. The new parameters are provider, providerid, and g-transid. For more information, see Authenticating and authorizing through a redirect URL.

No hay mucho que comentar aquí. Lo único que no sé cuán utiles seran esos valores para un tercero.

## OAuth scope can be modified by third-party responses

> You can configure an external server to override the API scope value. For more information, see: Scope.

Sinceramente en una primera lectura no tengo ni idea de para que sirve esto ni en que escenarios es útil. Tocará leer con detenimiento y probar.

## Preventing browser CORS alerts in the Test tool

> The API Designer Test tool sends requests from the browser that can trigger CORS alerts. To prevent CORS alerts, the Enable Proxy check box is provided to send test messages from the server that hosts API Designer rather than from the browser. For more information, see Testing an API with the API Designer test tool.

Pues no veo nada sobre "Enable Proxy check box" en el enlace al que remiten. Seguramente aún no hayan actualizado la documentación. En cualquier caso, parece que se refieren a que las llamadas no se harían desde el navegador y se evitarían los problemas de CORS.

## Revoke single OAuth tokens

> If you are using the DataPower® Gateway, you can now revoke a single OAuth token for an application. For more information, see Creating an OAuth provider API.

Bien, antes tenías que revocar todos los tokens de una aplicación cuando uno se veía comprometido. Atención a las limitaciones que indican en Bluemix público.

## Secure APIs with third party OAuth instead of Mobile First Foundation

> Secure your API with a third-party OAuth provider instead of the IBM MobileFirst™ Foundation authorization server. For more information, see Integrating third party OAuth provider.

Algo muy pero que muy necesario. Ahora hay que ver como funciona, en la versión 5.0.7 también se hablaba de esto y no funcionaba.

Hay que indicar también que la integración con un *Auth Server* externo es solo parte de la solución. Para dar una experiencia cómoda a los desarrolladores, los *client_id* que te proporcione el *API Portal* tiene que ser lo mismo que use el *Auth Server*, por lo que esta integración sigue siendo un punto a resolver.

## Secure your APIs with OpenID Connect

> You can secure your APIs with OpenID Connect(OIDC) by using a pre-supplied sample OAuth Provider API that you customize in accordance with your OIDC configuration. For more information, see Securing your APIs with OpenID Connect.

Pues de ser verdad sería un gran avance para el producto. Ahora, como siempre, falta ver que funciona de verdad y que no está limitado al servidor OAuth de caja y se puede usar con proveedores de terceros.

## SOAP update action no longer overwrites the API 

> When you update a SOAP API from a WSDL definition, only those sections of the API that are affected by the new WSDL are replaced, the other sections are unchanged. In previous releases, the update action completely overwrote the configuration of the SOAP API definition, including all design properties and assembly configuration. For more information, see Updating a SOAP API.

Mira, paso, no entiendo como se sigue dando soporte a SOAP. REST rulez!

## Use Honeypot for spam protection in the Developer Portal

> Honeypot protection provides security mechanisms to protect your Developer Portal site from form submission by spam bots. If spam bot activity is detected, form submission is blocked. For more information, see Using Honeypot for spam protection.

Bien, todo elemento de protección es bienvenido. Pero, vamos que parece un mecanismo sencillo para evitar bots genéricos. Alguien que quiera atacarnos expresamente mirará cuáles son los campos a rellenar e ignorará los ocultos en el bot que se monte ¿no?

## Using the Views module in the Developer Portal

> Create new views in the Developer Portal, such as content lists of Products, APIs, and applications, by using the Views UI module. For more information, see Using the Views module in the Developer Portal.

Pues habrá que verlo, por la descripción puede ser algo realmente potente o una cutrez impresionante. Insisto, si aporta valor, pondría la decisión cada vez más difícil a los que se plantean montar su portal y no usar el de caja.

## View cluster information by using Elasticsearch REST API calls

> You can use Elasticsearch API calls to view a health status of red, yellow, or green for your identified clusters. For more information, see Obtaining cluster health information by using REST API calls.

Aún no he mirado esto, pero iré actualizando el texto conforme vaya revisando las novedades.

De todos modos, todo el tema de la gestión de clústeres en el *Manager* se tendrá que replantear. Si vamos a tener contenedores efímeros de Datapower, no tiene sentido que el manager los gestione como ahora, considerando que son máquinas siempre disponibles y estables. ¿qué pasa si levantas unos cuantos Datapower para atender un pico y luego los paras? ¿estarán dados de alta en el manager y las analíticas te saldrán en rojo? 

Otros enlaces que tengo pendiente de comentar son estos:
https://developer.ibm.com/apiconnect/whats-new/
https://developer.ibm.com/apiconnect/2017/06/30/see-future-apim-datapower-api-gateway-tech-preview/