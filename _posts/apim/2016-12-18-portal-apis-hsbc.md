---
title: "HSBC Developer Portal"
categories:
  - APIm
tags:
  - APIm
  - HSBC
  - "API Portal"
  - PSD2
excerpt: "HSBC publica una versión Beta de su portal del desarrollador, incluyendo 3 sencillas APIs: Localizador de oficinas, localizador de cajeros e información de productos"
header:
  teaser: /assets/images/HSBC-logo.teaser.png
---

Me parece muy acertado el camino seguido por HSBC en su estrategia de APIs al salir inicialmente con un [portal](https://developer.hsbc.com/) con solo 3 APIs sencillas:

* Localizador de oficinas
* Localizador de cajeros
* Información de productos

Todas ellas usan información libre y gratuita que ya estaban dando seguramente en su web, por lo que pueden exponerlas sin tener que preocuparse de desarrollar complicados mecanismos de seguridad o facturación, ni tener que comprometerse con SLAs. Toda la experiencia adquirida les será muy útil para abordar en el futuro APIs más complejas donde:

## Se requerirán modelos de seguridad más complejos.

Por ejemplo, para consultar las cuentas, tendrían que incluir mecanismos OAuth o similares para que el propietario de la cuenta conceda autorización a la aplicación que invoca al API a acceder a los datos de la cuenta (todo ello sin facilitar las claves personales del propietario de la cuenta a la aplicación). Un fallo en estos mecanismos de seguridad (o en la integración con otros sistemas de seguridad del banco) sería muy grave, ya que estamos hablando de que la información de las cuentas de usuarios del banco quedaría expuesta y comprometida. 

Sin embargo, un API que accede a datos libres como la información de oficinas puede ser publicada simplemente con un mecanismo basado en API Key, que permite tener registro de las aplicaciones que llaman y controlar su consumo. Seguramente HSBC tenga soporte de caja en el producto de gestión de APIs que estén usando y no necesiten ninguna compleja integración con sus sistemas para ello. Además, ¿qué puede pasar si esto falla? ¿qué una aplicación consiga acceder al listado de cajeros sin estar suscrita a ese API? ¿qué una aplicación consiga realizar más consultas al listado de cajeros que la establecida? Este tipo de APIs son los candidatos perfectos para realizar las comprobaciones y ajustes de estos sistemas básicos de control.

Para vuestra tranquilidad, es por estos temas que la PSD2 cuenta con mecanismo de investigación y revisión tanto de proveedores de APIs como de las aplicaciones que las consumen. Vuestros datos y cuentas estarán a salvo si se cumple la normativa :smirk:.

## Será necesaria monetización

Ya que el coste para mantener estas APIs será mayor y la información que ofrecen más valiosa como para regalarla. Y si pagan por ello, los desarrolladores van a exigir calidad y serán muy críticos ante pérdidas de servicio.

Por contra, la información de cajeros ya la estaban dando gratis en la web (y por otros medios) y el coste para ofrecerla vía API no les resultará muy alto. Si no les supone coste, los desarrolladores seguramente serán más comprensivos antes los fallos e indisponibilidades del servicio comunes en toda nueva plataforma hasta que se estabiliza.

## El otro camino

Si no quieres seguir el ejemplo del HSBC en tu gestión de APIs e intentas salir desde el inicio con todas las APIs que vas a ofrecer, tendrás que implementar todos estos mecanismos extras e integrarlos con tus sistemas y realizar pruebas exhaustivas para salir con garantías. Durante este tiempo la competencia estará ya ganándose la confianza de los desarrolladores y atrayendolos a sus plataformas y escuchando sus peticiones y adquiriendo experiencia real. Recuerda que en gestión de APIs, **los desarrolladores son tus clientes**. Igual cuando llegues con tus APIs perfectas y tu plataforma completa (o que tu crees completa), los desarrolladores estén ya contentos con otros proveedores y te va a costar convencerles de que se conviertan en clientes tuyos. Además, ¿cómo piensas convencerles? ¿conoces sus necesidades? Ah, no, que no has estado trabajando con ellos y escuchándolos porque estabas ocupado con tu megalómano cojoproyecto. 

Imagina, por ejemplo, que las credenciales de la aplicación de un desarrollador se ven comprometidas, y en la practica tu mecanismo de revocación y regeneración es inexistente o resulta ineficiente o engorroso. Si detectas este problema con un API gratuito y lo corriges el impacto no será muy grave. Pero si te enfrentas a este problema con un API de pago, tienes un problema y es una comunidad de  desarrolladores descontenta que seguramente no quiera seguir usando tu plataforma (y con razón).