---
title: "Primeras pruebas con DeferPanic"
categories:
  - Serverless
tags:
  - Serverless
  - Unikernels
  
excerpt: "Pruebas con la plataforma de unikernels DeferPanic"
header:
  teaser: /assets/images/Alfredo_Landa.teaser.jpg
---

Tengo pendiente escribir una introducción a los **unikernels** y porque me interesan en relación al mundo Serverless.

Mientras que encuentro tiempo, si quiero al menos guardar las notas de unas pruebas en la plataforma [DeferPanic](https://deferpanic.com) en la que estoy experimentando sin tener que montar todo un ecosistema de herramientas en local.

En el [*dashboard*](https://deferpanic.com/home/images) creo una nueva imagen con todos los valores por defecto, excepto:

* Nombre: Test
* Build from Github
* Compiler: rumprun
* Programming language: NodeJS
* Source(github): https://github.com/deferpanic/nodejs_example

Después de un tiempo, en la misma página de imágenes, aparecerán en listado con un ID y en el campo *Build Status" con valor "Success"

En la página de detalle de la imagen (en mi ejemplo con ID 456), https://deferpanic.com/home/images/show/456 se puede descargar la imagen.

Tengo que probar a arrancar esa imagen en KVM e Hyper-V, pero mientras dando al botón *Launch* se crea una [instancia](https://deferpanic.com/home/instances) en el IaaS de DeferPanic. 

El detalle se puede ver en una URL de este tipo https://deferpanic.com/home/instances/show/3067 , donde se  pueden ver los logs del arranque del kernel y que la IP que da DeferPanic es privada, aunque hay posibilidad de añadir otras instancias a esa misma red.




