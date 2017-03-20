---
title: "La experiencia de programar un bot en OpenWhisk"
categories:
  - Serverless
tags:
  - Serverless
  - OpenWhisk
  - APIConnect
  - Telegram
  - Bluemix
excerpt: "Reflexiones sobre la experiencia de programar un bot sencillo para Telegram con OpenWhisk en Bluemix"
header:
  teaser: /assets/images/apache-openwhisk.teaser.svg
---

Tenía pendiente de hace tiempo escribir sobre la experiencia de programar el [bot de la lotería de Navidad con tecnología Serverless][bot-loteria-post] y por fin he sacado algo de tiempo

[bot-loteria-post]: {{ "" | absolute_url }}{% post_url serverless/2017/01/2017-01-07-bot-telegram-sorteo-nino %}


### El reencuentro con OpenWhisk en Bluemix

Probé un poco OpenWhisk cuando todavía estaba en BETA, y una vez en GA me encuentro con la sorpresa agradable de que ahora el cliente de línea de comando es un simple ejecutable (creo que en Go). La verdad es que me parece mucho más comodo y simplifica el empezar a trabajar con la tecnología. Entiendo las ventajas de determinados lenguajes como Python a la hora de realizar prototipos, pero resulta engorroso que para empezar a programar mis acciones/lambdas en JavaScript tenga que tener instalado Python y una gran cantidad de librerías adicionales que le habrán facilitado mucho la vida al programador de la interfaz de línea de comando, pero que yo no necesito ni quiero para nada. Y eso sin contar con la cantidad de versiones e incompatibilidades (oh, tienes Python X.Y pero para usar este CLI necesitas la versión Z.W . Oh, al instalar la nueva versión, te dejan de funcionar no sé cuantas otras aplicaciones y librerías que iban bien en la otra versión). Así que mucho mejor ahora que instalar el CLI de OpenWhisk es [bajar un único ejecutable](https://openwhisk.ng.bluemix.net/cli/go/download/).

Por otro lado, la [GUI](https://console.ng.bluemix.net/openwhisk/editor) solo sirve para unos primeros pasos. Muchas de las funcionalidades solo están disponibles desde la línea de comando.

#### ¿y fuera de Bluemix?

Pues a nivel profesional, la verdad es que las ventajas de tener OpenWhisk en tu CPD local o en tu nube privada son menos claros que en nube de un proveedor externo. Quizás para probar antes de pasarte a la nube...
Como desarrollador te puedes [montar](https://github.com/openwhisk/openwhisk/) tu propio entorno y se dispone de Vagrant y Ansible, pero por ahora no los he probado (dudo que en un equipo de 8 gigas puedas hacer mucho)

### ¿otro API Gateway? ¿a qué juegas IBM?

No he probado ningún caso real con Amazon, pero me da la impresión de que todo parece un diseño más redondo, como más integrado. Así, si quieres acceder de forma fácil y controlada a tus lambda, estas se pueden exponer con el API Gateway.

Pero en IBM te encuentras que el producto está basado en Datapower y este es el API Gateway por defecto. Pero en [la versión 5][post-version-5] y tras la compra de StrongLoop también es posible usar un microgateway basado en node. Lo malo es que al definir un API tienes que decidir de antemano si quieres desarrollar tu API para Datapower o para el microgateway (Ya el hablar de "desarrollar" tu API suena mal cuando debía ser una definición y el desarrollo hacerse solo en el backend).
No contentos con esta situación en IBM añaden un nuevo API Gateway a la ecuación con la nueva facilidad experimental de [exponer *actions* como servicios REST](https://www.ibm.com/blogs/bluemix/2017/01/exposing-openwhisk-restful-apis-api-gateway/)

[post-version-5]: {{ "" | absolute_url }}{% post_url apim/2016-03-28-novedades-api-connect-5 %} 

Obviamente este nuevo componente puede considerarse como algo conforme al patrón "API Gateway", pero en ningún caso tiene todas las características que da una solución de gestión de APIs. ¿no habría sido mejor integrar OpenWhisk con el API Connect del propio IBM?

La interfaz REST por defecto de OpenWhisk, aparte de requerir un formato de entrada/salida peculiar, se basa en una autenticación básica, por lo que no podemos usar estos *endpoints* en los clientes, ya que expondríamos nuestras credenciales de OpenWhisk. La exposición del nuevo API Gateway experimental elimina la autenticación básica y la cambia por una exposición pública. Pero, repito de nuevo, ¿no hubiese sido mejor integrar OpenWhisk con API Connect?  

¿no deberían en IBM hablarse un poco más entre ellos? Ser grandes no es solo tener muchos empleados y departamentos, también es tener una visión clara y unificada de su estrategia y arquitectura.

Mis pruebas fueron en enero y tengo este texto en borrador desde entonces. Supongo que tras revisar a fondo esta [noticia](https://developer.ibm.com/apiconnect/2017/03/17/coming-soon-30-seconds-serverless-traditional-managed-apis/) pueda cambiar mi interpretación de la estrategia de IBM.
{: .notice--info}

### Publicar en API Connect

Si descartamos el uso del API Gateway experimental de OpenWhisk, parece lógico que intentemos proteger nuestras *actions* de llamadas indeseables. 

* Con API Connect podemos añadir un client-id que podemos reiniciar en caso de verse comprometido. Con el API Gateway experimental de OpenWhisk tendríamos que cortar la publicación y volver a publicar con una nueva URL afectando a todos los clientes que tengamos. 
* Y aunque el paradigma serverless nos ayuda a escalar sin problemas, no deja de tener costes. Al menos para pruebas, no queremos encontrarnos sorpresas por atender un aluvión de peticiones no esperadas. Con API Connect podemos establecer un plan de consumo que nos evite atender más llamadas al mes de lo que nuestro presupuesto de pruebas nos permita. Eso sí, aunque no incurramos en coste en la parte Serverless, seguiremos teniendo que pagar el número de invocaciones al API.

Puedes ver la definición del API (y un producto de prueba) en [GitHub](https://github.com/luzbel/LoteriaTelegramBot/tree/dev/API%20Connect)

Como puedes observar:
1. Intenté validar el esquema de la [petición que envía Telegram](https://core.telegram.org/bots/api#update) pero el contenido es variable. Tengo que seguir probando como realizar estas validaciones con esquemas complejos en API Connect, por lo que está validación está dentro de un "if false" para evitar temporalmente su ejecución.
2. Como el único medio de hacer llegar parámetros a una *action* en OpenWhisk es a través del cuerpo del mensaje, he utilizado una política *GatewayScript* en la que añado los campos que quiero pasar al JSON recibido desde Telegram. No estoy nada contento con esta aproximación, porque me obliga a procesar todo el cuerpo en JavaScript e imagino que no será viable si recibimos *payloads* largos como fotos u otros objetos multimedia. Supongo que una XSLT será más óptimo, aunque no descarto que exista algún otro mecanismo alternativo en OpenWhisk para pasar este tipo de configuraciones.
  * En cualquier caso, uso este sistema para pasara a la *action* la URL a la que tendrá que conectar (y que es diferente para consultar la lotería de Navidad o la del Niño), y para pasar el token del Bot de Telegram
3. Uso una política proxy simple para invocar a OpenWhisk, declarando las variables de autenticación básica como propiedades del API
  * Aunque API Connect permite codificar los valores parametrizados para que no se vean en claro, no me siento muy cómodo con esta ofuscación básica. Al fin y al cabo un API exportado en formato Swagger se tiene que poder importar en cualquier API Manager y publicar en cualquier catálogo, por lo que debe tratarse de un hash simple con una clave común a todas las instalaciones de API Connect. 
4. Al salir del proxy podríamos devolver directamente la respuesta de OpenWhisk, pero no me parece buena idea. Me parece mejor aprovechar el API para abstraer al cliente del *backend*. Así si el día de mañana cambiamos de OpenWhisk a , por ejemplo, Azure functions, el cliente no tendría ni que enterarse. Así que monto un mensaje a medida de respuesta y si alguien consiguiera hacerse con el client-id de forma fraudulenta al menos no le daríamos pistas de cual es la tecnología de vuestro *backend* y no dándole facilidades para su ataque. Cara a Telegram es igual, ya que seguramente ignore el cuerpo del mensaje y sólo estará interesado en el *status code* que le devolvamos (podríamos simplemente borrar el cuerpo de la respuesta antes de devolverla). Por último, esto es una primera aproximación y en un escenario real de producción deberíamo ver y eliminar también cualquier cabecera que pudiese identificar al *backend*.

### La *action*

Podéis darle un vistazo al código en [GitHub](https://github.com/luzbel/LoteriaTelegramBot/blob/dev/OpenWhisk/telegramLoteriaBot.js) y la verdad es que tiene bastante poco que comentar, una llamada al [API de El País](http://servicios.elpais.com/sorteos/loteria-navidad/api/) para consultar el premio del número y otra al API de Telegram para devolver la respuesta.

### La parte negativa

* El [bug](https://github.com/openwhisk/openwhisk/issues/252) que impide usar caracteres con acentos y eñes en OpenWhisk y que obliga a un *ugly hack* que por supuesto no deberíamos llevar a producción. En cierto modo, demuestra la **inmadurez** de la plataforma.
* ¿dónde están las analíticas para ver el detalle de las invocaciones? ¿solo se tiene acceso al tiempo medio de respuesta de hasta 200 ejecuciones? De nuevo algo bastante lejos de lo necesario para un uso productivo.
  * Como alternativas (leáse *workarounds*) [procesar los logs](http://jamesthom.as/blog/2016/10/31/serverless-logs-with-elasticsearch/), pero es que esto me lo debía dar la propia plataforma.
  * ¿se podría consultar la BD interna de OpenWhisk con el histórico de ejecuciones? Igual es una aproximación interesante de estudiar
* Cada vez que se borra una acción y se recrea se pierde el versionado. Desde mi punto de vista el versionado tiene que estar en tu git, y el versionado que te hace OpenWhisk es innecesario.

### Conclusiones
* La plataforma OpenWhisk está todavía algo inmadura, pero promete.
* La integración con API Connect dejaba en enero mucho que desear a la espera de próximas novedades y mejoras por parte de IBM.