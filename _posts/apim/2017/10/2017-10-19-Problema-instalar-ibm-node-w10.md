---
title: "Problema instalando el node de IBM"
modified: 2017-10-23
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
  - Node
  - LaunchAnywhere
excerpt: "Problema con LaunchAnywhere al instalar la versión de Node de IBM requerida por el toolkit de API Connect"
header:
  teaser: /assets/images/apiconnect_logo.png
---

No entiendo que gana IBM con sacar una versión de Node. ¿no les vale la oficial? ¿qué tiene de especial? En su propia [página](https://developer.ibm.com/node/sdk/v6/) dicen que es equivalente a la 6.11.3 de la comunidad. Encima su instalador me daba el siguiente error

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/ibmnode/ErrorLaunchAnywhere.png){: .align-center}

Después de googlear (otra vez que olvido duckduckgoear) y múltiples intentos de ajustar el PATH para que funcionase, llego a que la solución de un problema con IBM me la da CA :expressionless: en este [enlace](https://support.ca.com/us/knowledge-base-articles.tec1751304.html) en el que se permite pasar como argumento a un instalador con LaunchAnywhere la ubicación de la VM de Java.

Vamos, que hay que invocar al instalador con algo así

```sh
ibm-6.11.3.0-node-v6.11.3-win-x64.exe LAX_VM "c:\Program Files\Java\jre1.8.0_144\bin\java.exe"
```

```sh
c:\>node -v
v6.11.3

c:\>npm -v
3.10.10
```

De que npm es un infierno y del resto de problemas que da instalar el APIc toolkit hablamos otro día :angry:


