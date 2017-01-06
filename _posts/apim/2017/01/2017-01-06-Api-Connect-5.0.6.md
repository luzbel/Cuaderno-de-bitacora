---
title: "Novedades API Connect 5.0.6"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
  - SNI
  - OpenShift
excerpt: "Lista de novedades de IBM API Connect 5.0.6, incluyendo soporte parcial a SNI e imágenes preliminares de Docker para el API Manager y el API Portal"
header:
  teaser: /assets/images/apiconnect_logo.png
---

Lista completa de [novedades](http://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.overview.doc/overview_whatsnew.html?lang=en) de IBM API Connect 5.0.6.

Hablemos de 2 de ellas, SNI y Docker:

## SNI

API Connect 5.0.5 tiene 2 importantes carencias en cuanto a soporte [SNI](https://es.wikipedia.org/wiki/Server_Name_Indication):

### El API Gateway no soporta ser un cliente SNI
Si usas Datapower como Gateway, hasta la versión 5.0.6 no podías invocar correctamente servicios que hiciesen uso de esta extensión de TLS para presentar su certificado.

Si tus servicios de backend están desplegados en OpenShift, por ejemplo, podrás llamarlos mediante tráfico seguro (https) solo si te vale que se presente el certificado por defecto del *router* de OpenShift. Si necesitabas que tu servicio presentase un certificado particular, el API Gateway de la versión 5.0.5 no era capaz de realizar la negociación SNI y siempre recibía el certificado por defecto, en vez del particular del servicio. 

Lo curioso de esto es que Datapower tiene soporte SNI desde hace bastante tiempo, y era bastante absurdo que el API Gateway funcionando sobre Datapower no tuviese soporte SNI. Si te parece surrealista, espera, que hay más, el propio IBM en una [nota técnica](http://www-01.ibm.com/support/docview.wss?uid=swg21699392) explicaba que se deberían usar las nuevas conexiones con soporte SNI en Datapower, en vez del objeto equivalente obsoleto sin soporte SNI. Puedo entender que un proyecto antiguo desarrollado sobre Datapower no esté actualizado, pero siendo el API Gateway un desarrollo reciente y con un nivel de evolución y actualización supuestamente elevado, el que use un objeto obsoleto no recomendado por el propio IBM parece bastante absurdo.

En cualquier caso, **parece**(no lo he probado aún) **que al fin se ha solucionado este problema en la versión 5.0.6**, e incluso se dispone de la posibilidad de deshabilitar SNI para el hipotético caso de que nuestro servicio de backend fuese incompatible con esta extensión de TLS. 

Como puedes ver en esta [solicitud de mejora del producto](https://www.ibm.com/developerworks/rfe/execute?use_case=viewRfe&CR_ID=92889) IBM era consciente del problema desde al menos Agosto y han tardado **¡¡¡ 4 meses !!!** en resolver algo que un *pull request* en cualquier proyecto OSS hubiese solucionado en horas. Tenlo en cuenta la próxima vez que tu comercial de IBM te venda la moto de su soporte y ritmo de evolución :stuck_out_tongue_winking_eye: 

### La CMC no soporta ser un cliente SNI

Como puedes ver en esta [solicitud de mejora del producto](https://www.ibm.com/developerworks/rfe/execute?use_case=viewRfe&CR_ID=92900) la CMC del API Manager no soporta SNI. Si has montado tu Datapower en OpenShift usando la versión [Docker](https://hub.docker.com/r/ibmcom/datapower/), no podrás añadirla a un clúster de API Gateways. IBM debe pensar que en las empresas jugamos con los dockers pelados como los estudiantes en sus casas y no los tenemos debajo de un PaaS como OpenShift, porque no son necesarias todas las facilidades que proporcionan. Así que 4 meses después de solicitar la mejora, seguimos sin tener esta funcionalidad por lo que el contenedor Docker de Datapower es totalmente inútil para usuarios de OpenShift.

## Docker

IBM sigue con su estrategia de llevar su plataforma de gestión de API a Docker (personalmente me gustaría más ver una versión [KVM](https://www.ibm.com/developerworks/rfe/execute?use_case=viewRfe&CR_ID=92954) de todas las *appliances* de API Connect y no soy el [único](https://www.ibm.com/developerworks/rfe/execute?use_case=viewRfe&CR_ID=93631)).

Inicialmente sacaron una desastrosa versión de su Datapower (tamaño descomunal para un contenedor, requería privilegios de administración comprometiendo todo el *host*, bloqueo de recursos del *host* en paradas no controladas , etc. Vamos, algo totalmente imposible de llevar a producción). Su segunda versión mejoró bastante eliminando todos los inconvenientes e incluso sorprendiendo sacando una edición gratuita para desarrolladores (realmente sorprendente en un producto como Datapower). Y aunque ya se trata de algo preparado para llevar a producción, Datapower no deja de ser un componente pesado, monolítico y que me cuesta ver en entornos de contenedores ligeros. Ve y pregunta al administrador de tu PaaS a ver que opina de que cada instancia de Datapower necesite 8 gigas de RAM.

Pero hoy estoy hablando de los  [Dockers](https://hub.docker.com/r/ibmcom/apiconnect/) que han liberado en la versión 5.0.6 para el API Manager y el API Portal. Lo primero que hay que remarcar que son aptas **"solo para desarrollo"**. Y lo pongo entre comillas porque me gustaría saber cuantos desarrolladores disponen de equipos con toda la RAM necesaria para levantar una instalación completa bajo Docker de todos los componentes necesarios de una instalación de API Connect: 8GB para el manager (16 si quieres hacer alguna prueba en clúster, que igual ni está permitido en la licencia de desarrollo, o si quieres probar alguna migración o *upgrade*), otros 8GB para el Datapower, ¿4GB? para el portal y todo lo que te pida tu sistema base (docker + APIc developer toolkit + sistema operativo).  

Lo segundo es que IBM va a tener que trabajar mucho en convertir algo que venía en una *appliance* monolítica en un conjunto sincronizado de contenedores ligeros usables en un PaaS. Soy bastante malo con las predicciones, pero sinceramente no creo que tengamos versiones productivas de Manager y Portal en Docker a corto plazo. 

No he probado aún los contenedores pero por lo que veo en [GitHub](https://github.com/strongloop/apiconnect-docker) ya han sacado elasticsearch y logstash del API Manager. Empezar por estos componentes me parece bastante acertado, ya que desde hace tiempo tengo la impresión de que el tener las analíticas incrustadas en la *appliance* del Manager penaliza bastante en su rendimiento, consumo de recursos y estabilidad. Si estos componentes están en contenedores aparte, podrán escalar de forma independiente cuando tu tráfico aumente y se genere un mayor número de analíticas.







