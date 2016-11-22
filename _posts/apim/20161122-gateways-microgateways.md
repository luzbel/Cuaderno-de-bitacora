---
title: "Gateways y microgateways"
categories:
  - APIm
tags:
  - APIm
  - APIGateway
  - MicroGateway
excerpt: "Gateways y microgateways"
---

Cuando empecé a interesarme por la gestión de APIs, casi todos los productos ofrecían como uno de sus componentes básicos un gateway. 
En muchos casos, este gateway era la evolución de un producto anterior. Así CA partía de Layer 7, un reputado XML Firewall. IBM basaba su producto en Datapower, y WSO2 construía su Gateway aprovechando su arquitectura Carbon y usando su ESB como base.
En torno a este Gateway, se construía un "manager" o gestor de API y un portal enfocado al consumidor de las APIs. La capa de gestión estaba más o menos trabajada según el fabricante, llegando a ser casi inexistente.
En cualquier caso, el gateway era una pieza monolítica (generalmente por herencia del producto del que partía) y pensada para ponerse como frontal de los servicios. La idea era, por tanto, que un mismo gateway diera servicio a todas mis APIs. En todo caso, por motivos de resiliencia, aislamiento o rendimiento podría montar diferentes gateways (o clústeres de ellos) para diferentes propósitos. Por ejemplo, uno para el tráfico de internet y otro para el tráfico interno. O uno para atender las peticiones de un departamento grande/importante de la empresa y otro para el resto de departamentos.
[FALTA DIAGRAMA]

En esos tiempos, la única alternativa diferente era 3scale, que aparte de este modelo (que también incluía) permitía incrustar las funciones de control y monitorización del gateway en el código de tu servicio.
Con el paso del tiempo he ido descubriendo soluciones de gestión más ligeras. Por ejemplo, Kong usa nginx como base para su gateway, proporcionando una interfaz que facilita la configuración de este conocido proxy inverso. Tyk nos proporciona un gateway ligero construido en Go. Estos gateways son más ligeros, más escalables y seguramente más asequibles, por lo que una tendencia que llevo tiempo apreciando es que se tiende a acercar el gateway al servicio. Así, en Google Cloud Engine, puedes tener una ruta normal asociada a tu proyecto o puedes tenerla asociada a un API Gateway, que además de facilitarte el enrutado, te da el resto de ventajas de la gestión de APIs. RedHat ha comprado recientemente 3scale (no le debía ver mucho futuro a su apiman) y seguramente busque hacer lo mismo.
IBM, por su parte, ofrece desde la versión 5 dos sabores: Puedes seguir usando Datapower como gateway tradicional,  pero también ofrece un microgateway basado en node, fruto de la compra de StrongLoop (lástima que ahora mismo ambos sean incompatibles y no se puede mover un API de uno a otro facilmente). Así que seguramente seguiremos viendo un auge del uso de microgateways en los próximos meses.
[FALTA DIAGRAMA]
