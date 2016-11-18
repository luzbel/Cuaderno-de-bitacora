---
title: "¿usar ingeniería inversa para requisitos?"
categories:
- misc
tags:
- RequisitosSoftware
excerpt: "Viabilidad de usar ingeniería inversa sobre mi antiguo y no documentado sistema para obtener los requisitos para construir uno nuevo"
---

Me sorprende encontrarme en demasiadas ocasiones con proyectos no documentados, en los que ni los propios usuarios conocen su funcionamiento, y en los que se plantea realizar ingeniería inversa sobre el código existente para obtener los requisitos sobre los que construir una nueva versión.

La ingeniería inversa podía tener sentido en la época de los spectrum, msx, etc. con sus memorias de 64k. Recuerdo una historia de un desarrollador en ["The Untold History of Japanese Game Developers"](https://www.amazon.es/gp/aw/d/0992926025/ref=pd_aw_sbs_14_1?ie=UTF8&psc=1&refRID=MGAVC109BVDDMGPRFT9E) que aprendió a programar desensamblado un juego que intentaba copiar. 
Otro caso conocido fue el del clonado de la BIOS de los primeros IBM PC que supuso la explosión del mercado de los PC "compatibles" (Ver la serie ["Halt and Catch Fire"](http://m.imdb.com/title/tt2543312/) para una ficción del uso de la técnica del [diseño en sala limpia](https://es.m.wikipedia.org/wiki/Diseño_en_sala_limpia) o muralla china).
También puedo entenderlo para revisar una funcionalidad muy concreta, y ver , por ejemplo, como se realiza un determinado cálculo. Y remarco **revisar**, porque si los usuarios de negocio que definieron en su día el sistema, no conocen hoy como se realiza ese cálculo, tienes un grave y serio problema.

Veo además que una búsqueda de "reverse engineering eliciting requirements" en Google (ups, volví a olvidar usar DuckDuckGo) devuelve hasta artículos académicos sobre el tema, lo que me lleva a pensar que hay demanda del uso de esta técnica. 

Pero te voy a contar mi opinión subjetiva, y se resume en **NO LO HAGAS**. Lo siento, pero si no sabes y no tienes documentado como funciona tu sistema, estás muy, pero que muy jodido. Y es hora del "te lo dije". Y la ingeniería inversa puede quedar muy bien sobre el papel y funcionar en proyectos técnicos como Samba, dónde un montón de gente aburrida con mucho tiempo libre se pasa **años** buceando en un sistema y probando y teniendo versiones que **fallan** al subir un archivo. Pero tú no te puedes permitir que la nueva implementación de tu sistema devuelva el valor Y para un cálculo cuando antes devolvía X. Ni que en el flujo de tu negocio cuando antes un pedido pasaba por una pantalla de aprobación, ahora no lo pase, porque no se hayan interpretado correctamente unas condiciones en tu código original. Y me da igual que tu tengas el código original y los de Samba no y aún así triunfaron. Cualquier sistema medianamente complejo tendrá una cantidad considerable de código y no será un ejemplo de código estructurado, ordenado y legible cuándo estás en esta situación. 
Así que seguramente harás o encargarás una estimación de lo que costaría leer el código y como no podrás leer el código completo para evaluar su complejidad, tendrás que "jugar" a las estimaciones y decir cosas como "1 día por cada pantalla" o incluso afinando más "1 día por pantalla simple, 3 días por pantalla compleja", donde además de usar criterios subjetivos (¿qué es una pantalla grande? Si no sabes lo que hace ¿cómo saber que no esconde pantallas emergentes complejas? ) obviaras:

* la cantidad de trucos en el código y variantes que sorprenderán a los pobres desgraciados a los que le toque leer el código. Si, amigo, hay pantallas con 4 botones de aceptar, pantallas con métodos de inicialización de ¡3000 líneas! y una cantidad de código muerto que te hará perder mucho tiempo
* que por coste, contratarás a perfiles bajos, ya que no querrás quemar a tus desarrollador estrella con la tortura que supone pasarse horas intentando seguir el código de otro
* que estos desarrolladores no conocerán ni el código ni el sistema. Si ya uno de los principales problemas en el desarrollo es que el desarrollador no conoce tu negocio, pues sumale que no están procesando lo que un humano que si conoce tu negocio les transmite. Están trabajando sobre lo que interpretó otro y tradujo a un lenguaje no humano.
* que esa definición de lo que supuestamente hace tu sistema, llegará a otro desarrollador, igualmente ajeno a tu negocio

En resumen estás jugando al teléfono "escacharrado". En vez de que tú, que conoces tu negocio, **hables** con un desarrollador, vas a seguir este camino: Usuario negocio aplicación pasada->desarrollador versión antigua según su interpretación de lo que le contaron->código->nuevo lector e interpretación de lo que hace el código-> nuevo desarrollador implementado su interpretación de lo que le han contado. Vamos, la fórmula del fracaso absoluto. Une a eso el factor humano, muy propenso a **errores en tareas repetitivas y aburridas**. Engañate si quieres diciendo:
- que vas a usar a desarrolladores del código original o involucrar a tiempo parcial a usuarios de negocio para resolver dudas, pero va a existir un abismo entre lo que interpreta el que lee el código y el lenguaje de tu usuario de negocio.
- que vas a poder hacer entregas incrementales, cuando desconoces cómo funciona tu sistema y como partirlo

Te lo repito, vas a fracasar y sólo te salvará que un artículo similar se publique en la NewsWeek para que los CIOs dejen de creerse que estos proyectos son viables y los patrocinen.

Venga, vale, ahora leete [esto](http://casos-ingenieria-inversa.blogspot.com.es/2012/04/caso-final-de-analisis-ingenieria.html) u otros artículos similares y piensa que exagero, y que en el país de la piruleta las herramientas automáticas pueden "simplificar y acelerar enormemente la tarea de construir aplicaciones basadas en componentes, que incorporan código heredado". Adelante, te deseo lo mejor, suerte con tu proyecto de ingenieria inversa, ya me cuentas que tal.
