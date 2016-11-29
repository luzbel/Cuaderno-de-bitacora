---
title: "Nueva version BETA del Docker de Datapower"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
  - Docker
  - Datapower
excerpt: "Beta de la versión Docker de Datapower"
header:
  teaser: /assets/images/apiconnect_logo.png
---

Me entero por [twitter](https://twitter.com/UCZ443/status/760591394645872640) de que IBM ha subido a [Docker Hub](https://hub.docker.com/r/ibmcom/datapower/) una imagen de su Datapower. Su versión anterior estaba publicada en [GitHub](https://github.com/ibm-datapower/datapower-labs)

A primera vista:
1. El tamaño de imagen está por debajo de 1 Giga
  * Aunque IBM no proporciona el Dockerfile en github o docker hub (como si hacia en la versión anterior) al entrar por /bin/sh en el contenedor nos encontramos un Dockerfile y un entorno con busybox que nos indican que se ha montado desde scratch)
2. Parece no necesitar permisos privilegiados
  * Al menos no se ejecuta con --priviledged como la versión anterior y no parece necesitar bloquear recursos en el host. Antes el firmware se descifraba usando cryptodevices del host y en caidas incontroladas los /dev/crypt quedaban bloqueados e impedian iniciar una nueva instancia del contenedor
3. Se trata de una BETA de la 7.5.2 
  * ¿tiene sentido subir una BETA a dockerhub?

De llevarse bien con +IBM API Connect​ y poder crearse un dominio de API desde la CMC, se solucionarían la mayoría de problemas de la versión anterior que me llevaban a no recomendar su uso en entornos productivos y podría ser una opción interesante a valorar. Algunas cuestiones:
- Parcheado: Sin el dockerfile, se espera que las subidas a dockerhub estén alineadas con los fixpacks que libere IBM (en la versión anterior era un dolor actualizar la versión docker)
- Licencia: ¿cómo va a controlar IBM el número de instancias que levanto?
- Rendimiento: Suponiendo que la versión 5.0.3 de API Connect que se debe liberar en breve va a solucionar los graves problemas de rendimiento ¿como se comportará Datapower en Docker frente a la máquina física o virtual equivalente? 