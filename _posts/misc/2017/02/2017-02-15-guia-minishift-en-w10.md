---
title: "Guia instalación minishift en Windows 10"
modified: 2017-04-16
categories:
  - misc
tags:
  - OpenShift
  - MiniShift
  - PaaS
  
excerpt: "Guia instalación minishift en Windows 10"
---

Paso a limpio mis [notas][probando-minishift-post] sobre [Minishift](https://www.openshift.org/minishift/)

¿tienes solo 8 gigas de RAM en tu equipo de desarrollo, pero quieres tener tu entorno local lo más parecido a tu entorno real OpenShift?
Con 8 gigas olvidate de usar la imagen oficial de OpenShift, por lo que una buena alternativa es Minishift, aunque no es exactamente igual.

1. Lo primero es seguir las [instrucciones](https://github.com/minishift/minishift#installation) y empezar descargando la [versión actual](https://github.com/minishift/minishift/releases/tag/v1.0.0-beta.3), que en mi caso era [minishift-1.0.0-beta.3-windows-amd64.zip de https://github.com/minishift/minishift/releases/tag/v1.0.0-beta.3](https://github.com/minishift/minishift/releases/download/v1.0.0-beta.3/minishift-1.0.0-beta.3-windows-amd64.zip)
2. Descomprimir en un directorio, por ejemplo c:\minishift
3. Añadir al PATH el directorio de instalación y el directorio donde irá el cliente oc (%USERPROFILE%\.minishift\cache\oc\v1.4.1)
4. Comprueba que los valores por defecto (ejecutar en HyperV con 20 gigas de disco y 2 gigas de RAM) te encajan para las pruebas que quieres hacer
  * Si quieres cambiar algún parámetro, sigue la [documentación](https://github.com/minishift/minishift/blob/master/docs/using.md#persistent-configuration)
5. Arranca el "Administrador de Hyper-V" y asegurate de que no tienes ninguna máquina virtual llamada "OpenShift Origin" o "minishift" de alguna instalación anterior de OpenShift o minishift
6. Abre un "cmd" **como administrador** (no funciona con un usuario regular) e intenta ejecutar el comando "minishift start"
  * Si minishift no termina de arrancar y en el "Administrador de HyperV" entras en la máquina y ves que no hay actividad, seguramente esté usando cualquier tarjeta de red virtual que tengas por HyperV de otras máquinas
  * Puedes ver el detalle del problema [aquí](https://github.com/minishift/minishift/issues/265)
7. Para minishift desde la línea de comando con "minishift stop" 
8. Genera un nuevo "conmutador virtual" desde el "Administrador de HyperV" 
  * Acciones->Administrador de conmutadores->Nuevo conmutador de red virtual->Externo->Crear conmutador virtual
  * Pon un nombre a este conmutador
  * Tipo de conexion: Red Externa
  * Marca "permitir que el sistema operativo de administración comparta este adaptador de red"
  * Selecciona el adaptador de red físico que quieres usar en Minishift (cable o wifi)
    * Te interesa uno que tenga acceso a Internet para poder descargar y actualizar paquetes de software
9. Asigna esta conexión a la máquina "minishift" en el "Administrador de HyperV"
10. Vuelve a arrancar minishift con "minishift start" (como administrador). Si ya has superado los problemas, deberías ver algo así:

```
c:\minishift>minishift start
Starting local OpenShift cluster using 'hyperv' hypervisor...
 21.46 MB / 21.46 MB [=====================================================================================] 100.00% 0s
Provisioning OpenShift via 'C:\Users\pedroparra\.minishift\cache\oc\v1.4.1\oc.exe [cluster up --use-existing-config --host-config-dir /var/lib/minishift/openshift.local.config --host-data-dir /var/lib/minishift/hostdata]'
-- Checking OpenShift client ... OK
-- Checking Docker client ... OK
-- Checking Docker version ... OK
-- Checking for existing OpenShift container ... OK
-- Checking for openshift/origin:v1.4.1 image ...
   Pulling image openshift/origin:v1.4.1
   Pulled 0/3 layers, 3% complete
   Pulled 0/3 layers, 18% complete
   Pulled 0/3 layers, 34% complete
   Pulled 0/3 layers, 49% complete
   Pulled 0/3 layers, 65% complete
   Pulled 1/3 layers, 76% complete
   Pulled 2/3 layers, 86% complete
   Pulled 2/3 layers, 97% complete
   Pulled 3/3 layers, 100% complete
   Extracting
   Image pull complete
-- Checking Docker daemon configuration ... OK
-- Checking for available ports ... OK
-- Checking type of volume mount ...
   Using Docker shared volumes for OpenShift volumes
-- Creating host directories ... OK
-- Finding server IP ...
   Using 192.168.0.39 as the server IP
-- Starting OpenShift container ...
   Creating initial OpenShift configuration
   Starting OpenShift using container 'origin'
   Waiting for API server to start listening
   OpenShift server started
-- Adding default OAuthClient redirect URIs ... OK
-- Installing registry ... OK
-- Installing router ... OK
-- Importing image streams ... OK
-- Importing templates ... OK
-- Login to server ... OK
-- Creating initial project "myproject" ... OK
-- Removing temporary directory ... OK
-- Server Information ...
   OpenShift server started.
   The server is accessible via web console at:
       https://192.168.0.39:8443

   You are logged in as:
       User:     developer
       Password: developer

   To login as administrator:
       oc login -u system:admin


c:\minishift>
```

Ya puedes acceder a la consola de OpenShift desde https://{IP}:8443/console y acceder con el usuario developer/developer, y realizar acciones desde la línea de comando. Por ejemplo:

```
oc new-app https://github.com/openshift/nodejs-ex -l name=myapp
oc expose svc/nodejs-ex
```

[probando-minishift-post]: {{ "" | absolute_url }}{% post_url misc/2017/02/2017-02-10-probando-minishift %}

## Actualizo 16/04/2017

Parece que minishift tiene [problemas](https://github.com/minishift/minishift/issues/343) derivados de las limitaciones de red de HyperV. Ya no me gustaba que la VM estuviese asociada a un adaptador de red concreto (wifi o físico), encima cada vez que cambia la IP deja de funcionar. En una red casera tampoco es que vaya a cambiar mucho (seguramente solo si cambiamos de router), pero si que es un fastidio para demos que preparas en una red y luego no te funcionan al llevarla a otro.