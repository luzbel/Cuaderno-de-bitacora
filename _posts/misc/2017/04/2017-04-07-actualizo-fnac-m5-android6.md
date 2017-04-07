---
title: "Actualizo Phablet fnac M5 a Android 6"
categories:
  - misc
tags:
  - Fnac
  - BQ
  - M5
  - Android
  - Marshmallow
  
excerpt: "Movidas para actualizar un FNAC M5 a Android 6"
---

BQ [actualizó](http://www.mibqyyo.com/articulos/2016/05/18/android-marshmallow-novedades-bq/) el M5 a Marshmallow hace ya casi un año, pero los propietarios del dispostivo equivalente bajo la marca Fnac seguimos esperando. 

Me parece bastante mala política por parte de fnac, ya que independientemente de que hayan [dejado](http://www.economiadigital.es/directivos-y-empresas/bq-dispara-las-alarmas-tras-el-portazo-de-movistar-y-fnac_186310_102.html) de encargar sus teléfonos de marca blanca a BQ, hay clientes a los que tienen que seguir dando soporte. 

BQ, se da mus:

<blockquote class="twitter-tweet" data-lang="es">
  <a href="https://twitter.com/bqreaders/status/799540439560896512">November 18, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Así que no me queda otra que realizar una instalación de cero del *firmware* del BQ M5. Para ello, seguimos las instrucciones de BQ

* [Instalación](http://www.mibqyyo.com/articulos/2014/12/19/hard-reset-para-aquaris-e5-4g/)
* [Desbloquear el cargador de arranque](http://www.mibqyyo.com/articulos/2015/11/27/bootloader-dispositivos-bq)

## Desbloquear el cargador de arranque

Lo más destacable aquí es que no hubo manera de hacer funcionar los controladores USB proporcionados por BQ.

### Prueba y fallo 1

Forzar la instalación del controlador desde el menú contextual del archivo C:\Program Files (x86)\BQ Handset USB Driver\Win_64\android_winusb.inf

![Instalar]({{ site.url }}{{ site.baseurl }}/assets/images/fnacM5android6/forzar-driver.png)
{: .align-center}
{: .full}

Pero seguía sin reconecerse el dispositivo

### Prueba y fallo 2

Probé a [permitir la carga de dispositivos no firmados](https://answers.microsoft.com/en-us/insider/forum/insider_wintp-insider_devices/how-do-i-disable-driver-signature-enforcement-win/a53ec7ca-bdd3-4f39-a3af-3bd92336d248)

![Instalar]({{ site.url }}{{ site.baseurl }}/assets/images/fnacM5android6/dispositivos-no-firmados.png)
{: .align-center}
{: .full}

... Pero seguía sin reconecerse el dispositivo :sob:

![Instalar]({{ site.url }}{{ site.baseurl }}/assets/images/fnacM5android6/dispositivo-desconocido.png)
{: .align-center}
{: .full}

### Método alternativo

Pues nada, sigo el "método alternativo" indicado en el apartado final de las instrucciones de BQ que consisten en reiniciar el móvil pulsando el botón de encendido y el botón de subir volumen. Allí después de poner el móvil en modo "FastBoot" ya se puede desbloquear el cargador de arranque e instalar el nuevo *firmware*.

## Instalación

Al igual que en el caso anterior, tuve que poner el móvil en modo *fastboot* con el método alternativo, ya que en W10 no se reconocía el dispositivo.

## Conclusiones 

¡ Muchas gracias, Fnac ! Me encanta perder el tiempo en estas cosas que tú me deberías haber solucionado con una actualización OTA :rage:

Y no sólo es el rollo de tener que reinstalar, es que este método supone perder toda la configuración, volver a instalar las aplicaciones, volver a activar el 2FA en las páginas, hacer copia de seguridad de fotos antes, etc. 