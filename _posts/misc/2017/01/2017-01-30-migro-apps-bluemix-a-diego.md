---
title: "Migro apps a Diego en Bluemix"
categories:
  - misc
tags:
  - Bluemix
excerpt: "Migro apps a Dieego en Bluemix"
---

Si, debería usar Dockerfiles y tener mi entorno bien montado, pero solo quería bajar el cliente de forma rápida sin "ensuciar" el bash de Windows 10.

```
C:> docker pull ubuntu
C:> docker run -it ubuntu bash
# apt-get update
# apt-get install wget
# wget -q -O - https://packages.cloudfoundry.org/debian/cli.cloudfoundry.org.key | apt-key add -
# echo "deb http://packages.cloudfoundry.org/debian stable main" | tee /etc/apt/sources.list.d/cloudfoundry-cli.list
# apt-get install apt-transport-https
# apt-get update
# apt-get install cf-cli
# useradd -ms /bin/bash bluemix
# su - bluemix
```

La guía de IBM dice luego de añadir el plugin de migración a Diego, pero parece que el repositorio ya está configurado

```
$ cf add-plugin-repo CF-Community https://plugins.cloudfoundry.org/

FAILED
Plugin repo named "CF-Community" already exists, please use another name.
$ cf list-plugin-repos
OK

Repo Name      URL
CF-Community   https://plugins.cloudfoundry.org
```

Así que instalo directamente

```
cf install-plugin Diego-Enabler -r CF-Community
CTRL-D
```

Busco con docker ps -a el contenedor que acabo de cerrar y grabo la imagen para cada vez que necesite usar el cliente cloudfoundry con bluemix 

```
C:> docker commit 250ca065ce76 bluemix
sha256:51f184c6e859218a10200e56afd8848a7f57e53c324f91f41e737b0cc9f5990f
```

Y ya trabajo sobre esta image cuando quiero usar bluemix

```
C:> docker run -it bluemix bash
# su - bluemix
$ cf api https://api.eu-gb.bluemix.net
$ cf login -u [IBMID] -o [ORGName] -s [space]
```

Y empiezo a migrar, empezando por la que uso

```
$ cf dea-apps
$ cf enable-diego testIoTPStarter
$ cf restart testIoTPStarter
```

Como arranca bien,  sigo teniendo una sola app activa cada vez para evitar pasar del límite de consumo.
Primero migrando el resto de la región de Europa, y luego las de EEUU

```
$ cf migrate-apps diego
$ cf api https://api.ng.bluemix.net
$ cf login -u sebastian.blanes@gmail.com -o sebastian.blanes@gmail.com -s dev
$ cf migrate-apps diego
```