---
title: "Novedades API Connect 5.0.7"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
  - SNI
  - OpenShift
excerpt: "Repaso a la lista de novedades de IBM API Connect 5.0.7"
header:
  teaser: /assets/images/apiconnect_logo.png
---

En este [enlace](http://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.overview.doc/overview_whatsnew.html?lang=en) puedes ver la lista completa de novedades del recién liberado IBM API Connect 5.0.7. No he podido actualizar y probarlas aún pero, como siempre, escribo mis primeras impresiones y las dudas de temas que quiero comprobar:

## Analíticas 

> Analytics component has changed
>
> The Analytics component is now built using the Kibana V5.1 open source analytics and visualization platform. As a result, there are some visual and operational changes to dashboards and visualizations. For a summary of the key changes, see [The screen elements of a dashboard](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.apionprem.doc/capim_analytics_defaultdashboard.html?view=kc#capim_analytics_defaultdashboard__section_kibanav51). Other changes are highlighted within the relevant procedures for the analytics tasks.
>
> The event data that is generated in the API Connect on-premises cloud and displayed by the Analytics component can now be exported to third-party systems as a real-time data feed for centralized data consolidation, enhanced monitoring, and richer analytical data processing. The default ability to view and work with analytics data in the API Connect user interfaces is retained, but you can also now choose to disable access to analytics data within API Connect if preferred. For more information, see [Configuring destination targets for API Connect analytics data](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.cmc.doc/tapim_analytics_configuringanalytics.html?view=kc).

Ya [comenté][analiticas-post] algo sobre las novedades por venir en las analíticas. Que actualicen a una nueva versión de Kibana me parece genial, pero lo que tienen que hacer ya de una vez es "romper" el API Manager en distintos componentes y que todos estos elementos (Kibana, ElasticSearch, etc.) sean componentes aislados. Así podremos poner la versión que nos de la gana y escalarlos de forma independiente al resto de componentes del Manager. 

Por otro lado, sigo sin saber sin los *gateways* pueden enviar eventos directamente al componente de analíticas externo o si se sigue dependiendo de que se enriquezcan en el *API Manager*. Aunque no es información oficial, no me gusta nada lo que se indica en este artículo

> Gateway server sends analytics data back to the Management server. It discards the analytics data when the management server is down
>
> \- [Sunny Goel](https://blogs.perficient.com/integrate/2017/03/08/how-api-connect-components-interact-at-design-time-and-run-time/)

¿eso quiere decir que si se cae mi *Manager*, sigo dando servicio :thumbsup: , pero luego no podré monetizar y cobrar por su uso porque habré perdido los eventos :thumbsdown:?. ¿de verdad no aguanta los eventos durante un tiempo y realiza algunos reintentos el *Manager* antes de descartar? Al menos, hasta unos límites razonables. No es cuestión de que se caiga también el *gateway* (¡y dejar de dar servicio!:fearful:) por almacenar tantos eventos acumulados.
 
[analiticas-post]: {{ "" | absolute_url }}{% post_url apim/2017/03/2017-03-18-Api-Connect-analiticas %}

## Nuevo aspecto visual

> API Designer and API Manager have a new look>
> The API Designer and API Manager user interfaces have been restyled based on the [Carbon design system](http://carbondesignsystem.com/). This change affects only their "look and feel," not functionality.

Pos bueno, pos fale, pos malegro. Espero que no tenga que volver a buscar y aprender dónde se accedía a todas las funcionalidades.

## Monitorización para las aplicaciones Node

> Application metrics dashboard is now available for Node.js applications
>
> When you run a Node.js application (such as a LoopBack project) locally using the Developer Toolkit, you can view application performance metrics using the built-in application metrics dashboard. For more information see, [Viewing the application metrics dashboard](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.toolkit.doc/tapic_view_appmetrics.html?view=kc#task_wn3_4j3_cz).

Todavía no he jugado con la parte de "*Create*" de API Connect y considero que está bien tener una opción más, pero que cada cuál hará sus microservicios con el *framework* que quiera y no con el que te traiga tu producto de gestión de APIs. En cualquier caso, siempre es de agradecer cualquier monitorización, pero de nuevo cualquier instalación seria en producción ya tendrá su infraestructura de monitorización. Así que lo quiero es poder añadir esta información a mi sistema centralizado de monitorización y no depender de tener que mirar mis aplicaciones Node relativas a mis APIs únicamente en el cuadro de mandos que me proporciona API Connect.

## Múltiples clúster de gateways por catálogo

> Catalog supports multiple DataPower® Gateway services
>
> You can configure a Catalog to use two or more DataPower Gateway services. Then by modifying the Gateway service endpoints, and configuring your DNS appropriately, you can route API calls to the required Gateway service. For more information, see [Using multiple DataPower Gateway services with a Catalog](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.apionprem.doc/capic_multi_gw.html?view=kc).

¡¡¡ Siiiiii !!! Por fin. Tal y como dice la documentación, existen muchos escenarios en los que esta funcionalidad era muy esperada:

> * Load balance API calls across all the Gateway services.
> * Route API calls according to geography, so that calls are routed to the Gateway service that is most local to the calling application.
> * Control API calls according to application type; calls from development applications are made to different Gateway services to calls from production applications; for more information, see [Managing the application lifecycle](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.apionprem.doc/capic_app_lifecycle_manage.html)

## Muerte a los *Collectives* rarunos

> Collectives are deprecated in favor of Docker Swarm and Kubernetes containers
>
> IBM API Connect collectives are deprecated in IBM API Connect Version 5.0.7 in favor of container runtimes. For more information and background, see [Open, scalable, flexible runtime management of APIs through API Connect enabled containers](https://developer.ibm.com/apiconnect/2017/03/17/api-connect-enabled-containers/). For information on setting up and migrating to containers, see [Installing a containerized runtime environment](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.install.doc/tapic_install_container_env.html?view=kc).
>
> Existing customers can continue to use their collectives with IBM API Connect Version 5.0.7, and if wanted can expand their collective deployments to new servers. API Connect collectives are supported for existing customers until the end of support of IBM API Connect Version 5.0 (see [Software lifecycle page for IBM API Connect Version 5.0](https://www.ibm.com/software/support/lifecycleapp/PLCDetail.wss?q45=R102182Z60312V58). Until then, users of API Connect collectives are encouraged to migrate to container runtimes to take advantage of their agility and scalability.
>
> New customers should not install API Connect collectives because this feature is no longer supported for new users.

También es algo sobre lo que ya [escribí][collectives-post]. A la vista de la nueva documentación, añadiría:

* Todo el rato cambiando cosas que no son compatibles entre versión y versión y ahora nos ponemos medallitas y vendemos que se va a dar soporte a los que usan *collectives* ¿de verdad hay alguien usando *collectives*? Igual por eso lo dicen y quedan bien
* Normal que recomiendes lo nuevo, los *collectives* eran un engendro muy feo que nunca debieron ver la luz. 
* Bien, así que Swarm y Kubernetes. ¿y los que usamos un PaaS de verdad que nos abstraiga de esto como, por ejemplo, OpenShift? Viendo la documentación parece viable. Habrá que probar. 
* La [documentación](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.install.doc/Kubernetes_build_deploy_sample.html) solo habla de las aplicaciones construidas con StrongLoop/LoopBack. Pero , ¿qué pasa con el microgateway? Si, no deja de ser node, pero la documentación de la [instalación](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.install.doc/tapic_install_microgw.html) del microgateway sigue teniendo referencias a los *collectives*.

> This task assumes that you installed API Connect collective and registered the Member host with the collective, and that you added the API Connect collective to the Cloud Manager; for details, see [Installing API Connect collective](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.install.doc/tapic_install_collective.html?view=kc).

No lo entiendo :confused: ¿entonces los *collectives* desaparecen del todo o no? 

[collectives-post]: {{ "" | absolute_url }}{% post_url apim/2017/03/2017-03-23-fin-collectives-api-connect %}

## Arreglan cositas del *toolkit*

> Command-line tools now work with management server running on ports other than default 443
>
> If you change the TCP port number on which the API Management server listens, the apic command-line tool will now work properly if you specify the port with the command-line --server option.

Pos bueno, pos fale, pos malegro. Que todavía pasen estas cosas, que digo yo que con un poquito de OSS se solucionan en un plis. 

# Probar desde el *toolkit* con Datapower 

> Developer toolkit supports API testing with the DataPower Docker container
>
> When you test API from the Developer toolkit, you can now set an option to use the DataPower Gateway Docker container for a full set of security and policy capabilities. The toolkit synchronizes with the Gateway on save; you can now test product and plan level concepts; DataPower Gateway error logging and Request/Response logging are also integrated into the API Designer logging console.

Uhm, pues tiene muy buena pinta. Ya me he quejado varias veces del problema que supone que el *gateway* basado en Datapower y el *microgateway* no sean compatibles a nivel de ensamblado. Que un desarrollador trabaje con el *toolkit* en local no sirve de nada si desarrolla contra el microgateway y tenenmos planteado desplegar las APIs en producción sobre Datapower. Me gustará ver como se hace esta sincronización ya que si el *toolkit* mantiene un dominio en Datapower ¿qué le diferencia de un API Manager? ¿qué puedo ejecutar el *toolkit* en un equipo de desarrollo medio (un PC o portátil estándar) mientras que para usar el API Connect sobre Docker para desarrolladores necesito infraestructura externa dados los altos requisitos de memoria? 

Y volviendo a repetirme, la parte de edición (la sección *Draft*) debería desaparecer del Manager y quedar solo en el *toolkit*. 

## Extensiones Swagger en el *toolkit* 

> Developer toolkit supports vendor extensions
>
> API Designer now supports OpenAPI (Swagger 2.0) extensions (also referred to as "vendor extensions"). For more information, see [Adding an OpenAPI (Swagger 2.0) extension to an API definition (API Designer UI)](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.toolkit.doc/tapim_extensions_define_designer.html?view=kc). The command-line tool apic extensions command is also available for working with extensions. For more information, see [Toolkit command summary](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.toolkit.doc/rapim_cli_command_summary.html?view=kc#reference_ups_f3n_15) and [Extensions commands](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.toolkit.doc/ref_CLI_exten_commands.html?view=kc#extensionscommands).

Tengo pendiente y en borrador desde hace mucho una entrada sobre la importancia de trabajar y aprovechar las ventajas de las extensiones en Swagger (o sus equivalentes en otros lenguajes de extensión de APIs). Mientras no esté escrito, dejémosle en que es una buena mejora. Por cierto, ya que estamos hablando del *toolkit* ¿habrán alineado ya las versiones con el Manager o seguirá [Kraman](https://www.npmjs.com/~kraman) poniendo el número de versión que le de la gana?.

## Ciclo de vida de las aplicaciones  

> Implement lifecycle workflow for your applications
>
> By using the application lifecycle capability, you can have separate Development and Production endpoints for the same API. Applications that are subscribed to use the API initially have Development status, and can call the API only through Development endpoints. When application testing is complete, the application developer can request to upgrade the application to Production status; when the request is approved, the application is upgraded and can call the API through Production endpoints. For more information, see [Managing the application lifecycle](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.apionprem.doc/capic_app_lifecycle_manage.html?view=kc).

¡ Bien ! ¡yujuuu! Llevo pidiendo esto desde mi primer contacto con API Connect (cuando aún era IBM API Management v4) 

Como desarrollador de aplicaciones/consumidor de APIs quiero probar primero el API (en un entorno *sandbox* con datos ficticios sin coste para el productor ni para el consumidor de APIs) y luego usarlo de forma real. Esto se resolvía de forma bastante satisfactoria en otros productos (como WSO2 o CA), pero de manera bastante compleja y poca intuitiva para el consumidor de APIs en el producto de IBM (basicamente teníamos que entrar a un portal de sandbox, dar de alta una aplicación y suscribirnos al API, probar y luego volver a entrar en otro portal de producción y volver a crear una aplicación y suscribirse de nuevo al API). El nuevo mecanismo parece más intuitivo y además combina perfectamente con la nueva funcionalidad de tener distintos clústers de gateways en un mismo catálogo. 
 
> LoopBack 3.0 is now supported by API Designer and command-line tools
>
> When you create a new LoopBack project with the API Designer or apic loopback command, you now have the option of creating a LoopBack version 3.0 project. For more information on LoopBack 3.0, see loopback.io.

Pos bueno, pos fale, pos malegro. Como ya he dicho, implemento mis microservicios como quiero. Tengo que darle algún día una oportunidad a StrongLoop/LoopBack pero, por ahora, lo tengo en la lista de temas pendientes. 

> SNI in API Connect can be used with the DataPower Gateway
>
> To inject Server Name Indication (SNI) in communications between IBM API Connect and a DataPower Gateway, you set the hostname (rather than IP address). For more information, see [Adding a Gateway server](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.cmc.doc/create_node_gateway.html?view=kc).

¿En serio? ¿IBM vuelve a implementar algo que llevo tiempo [pidiendo](https://www.ibm.com/developerworks/rfe/execute?use_case=viewRfe&CR_ID=92900) y que ya [comenté][sni-post] que en la versión 5.0.6 no habían arreglado del todo? Voy a comprar un décimo de lotería porque estoy en racha.

Espero que con este cambio pueda desplegar Datapowers en formato Docker bajo OpenShift y gestionarlas desde un API Manager. No es algo que recomiende para producción (un Docker que consume 8 gigas de RAM no es algo que ningún administrador de sistemas te vaya a aceptar con facilidad), pero puede ser interesante para pruebas en desarrollo (y aprovechar la novedad antes mencionada de usarlos desde el *toolkit*). Si te decides a usarlos en producción, pues por un lado seguramente en un PaaS tengas una penalización en el rendimiento (¿red?) pero, por contra, podrás escalar más facilmente (la versión Docker de Datapower permite autoaceptar la licencia facilitando la automatización) lo cual compensará posibles temas de rendimiento. Me sigo preguntando como va a cobrar IBM este uso si mantiene un modelo de licenciamiento basado en el número de CPUs y estos contendores podrán ser efímeros y levantados sólo en momentos puntuales de mayor carga.

[sni-post]: {{ "" | absolute_url }}{% post_url apim/2017/01/2017-01-06-Api-Connect-5.0.6 %}

## Más allá del Q1 2017

Aparte de las mejoras de la versión 5.0.7, veo otro enlace de IBM sobre el [1Q 2017](https://developer.ibm.com/apiconnect/whats-new/) dónde también dejan caer futuras mejoras interesantes

### Optimizaciones 

> Performant & Flexible new API Gateway, built on DataPower
>
> Natively built API Gateway with low-level optimizations in the platform to provide increased performance with new built-in policies (Coming soon*).

La verdad es que el rendimiento del API Gateway es bastante malo para lo que puede dar por detrás Datapower en bruto. Veremos cuanto consigue optimizar IBM con respecto a la implementación actual del gateway y veremos qué impacto tendrá esto sobre las APIs existentes ¿tendré que reescribir los ensamblados de las APIs? ¿me valdrán mis políticas y extensiones? 

### Aislamiento (multitenant)

> API Connect workload isolation on DataPower physical gateways
>
> Run API Gateway services as isolated tenants with configurable CPU, memory file systems for operational resiliency (coming soon*).

Actualmente puedo montar distintos API Gateways sobre el mismo Datapower. Cada gateway correrá sobre su propio dominio y así estarán aisladas las APIs, claves de cifrado, etc. y un administrador de un dominio o usuario del API Manager o API **no** podrán acceder a los datos del otro dominio. 

Por ejemplo, imaginemos una gran empresa que quiere tener separados los APIs de distintos departamentos. A nivel lógico es posible creando diferentes organizaciones y catálogos, pero si se comparte el mismo clúster de Datapower  por debajo, las invocaciones a las APIs de un departamento podrían saturar el Datapower (justo arriba acabo de deciros que el API Gateway no es que rinda especialmente bien) y afectar a las llamadas a APIs del otro departamento. Supuestamente esta novedad nos permitiría tener un **multitenant real** con dominios con una capacidad reservada de CPU y espacio en disco.

Me pregunto por qué esta capacidad se ofrece solo para el Datapower físico. De primeras puede parecer que no tiene sentido para los virtuales ya que podríamos crear nuevas instancias de Datapower virtuales pero se me ocurren escenarios donde no estaría de más tener esta capacidad de aislamiento. Por ejemplo, en un API Connect sobre un Bluemix dedicado se tiene un único clúster de Datapowers. Por ejemplo podríamos querer reservar determinada capacidad (CPU, disco) para los gateways asociados a los catálogos de producción de modo que las pruebas de desarrollo nunca nos afecten.

### SwaggerHub

> SwaggerHub integration with auto-sync
> 
> Use SwaggerHub to design APIs and easily deploy to API Connect on Bluemix to speed API delivery.

Esto me lo apunto en mi lista de temas pendientes de revisar, porque también tiene buena pinta.

### API Management+OpenWhisk+CloudFoundry 

> Built-in API Management in Bluemix
> 
> Developers can build [serverless OpenWhisk and traditional Cloud Foundry apps](https://developer.ibm.com/apiconnect/2017/03/17/coming-soon-30-seconds-serverless-traditional-managed-apis/)  in Bluemix Public, and secure, control their APIs.

Sobre esto ya [comenté][openwhisk-post] algo y tendremos que estar atentos cuando aparezca esta funcionalidad para darle un nuevo repaso.

[openwhisk-post]: {{ "" | absolute_url }}{% post_url serverless/2017/03/2017-03-20-api-connect-openwhisk-cloud-foundry %}

### Otros temas

Se mencionan otros temas de los que he hablado últimamente, por lo que puedes dar un vistazo a este [recopilatorio][recopilatorio-post]

[recopilatorio-post]: {{ "" | absolute_url }}{% post_url apim/2017/04/2017-04-05-interconnect-2017 %}


