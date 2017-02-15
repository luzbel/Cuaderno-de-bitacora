---
title: "Probando minishift"
categories:
  - misc
tags:
  - OpenShift
  - MiniShift
  - PaaS
  
excerpt: "Probando minishift"
---

He formateado algo mejor las notas en esta [entrada][guia-minishift-post]
{: .notice--info}

[guia-minishift-post]: {{ "" | absolute_url }}{% post_url misc/2017/02/2017-02-15-guia-minishift-en-w10 %}

Nunca me ha gustado ir instalando cosas para probar hasta dejar el sistema inservible.
Ahora que el soporte a Docker en W10 ha mejorado, pensaba que manteniendo contenedores para cada experimento la experiencia mejoraría. No tengo todavía conclusiones, pero voy a seguir probando alternativas. Así que aquí van mis notas sobre la instalación de minishift (Un OpenShift reducido) sobre Hyper-V en un W10.

Hay que decir que intenté ejecutar la imagen oficial de OpenShift en HyperV, pero los 8 gigas de mi portátil no daban para ello.

Ojo, ya avisaba en el ["Acerca de"]({{ site.baseurl }}{% link _pages/about/acerca-de.md %}) del blog que mi intención no es hacer artículos bien formateados y escritos en plan tutorial. Solo intento registar a modo de cuaderno de bitácora las cosas en las que voy cacharreando. No esperéis el megatutorial de como instalar OpenShift y una valoración. Lo de dejarlo bien formateado, igual hasta un día me pongo.

## 2017/10/02

Sigo las instrucciones de https://github.com/minishift/minishift#installation

Descargo minishift-1.0.0-beta.3-windows-amd64.zip de https://github.com/minishift/minishift/releases/tag/v1.0.0-beta.3

Que raro, solo 6 megas

Descomprimo en c:\minishift

Creo c:\minishift\setpath con 
```
set PATH=%PATH%;c:\minishift
```

```
c:\ cd \minishift
c:\minishift setpath
```

como parece que los valores por defecto (HyperV , 20g disco y 2 gigas RAM) valen , arranco directamente

```
c:\minishift minishift start
Starting local OpenShift cluster using 'hyperv' hypervisor...
Downloading ISO 'https://github.com/minishift/minishift-b2d-iso/releases/download/v1.0.0/minishift-b2d.iso'
 40.00 MB / 40.00 MB [=====================================================================================] 100.00% 0s
E0210 23:33:27.920109    5968 start.go:135] Error starting the VM: Error creating the VM. Error with pre-create check: "Hyper-v commands have to be run as an Administrator". Retrying.
E0210 23:33:31.282927    5968 start.go:135] Error starting the VM: Error creating the VM. Error with pre-create check: "Hyper-v commands have to be run as an Administrator". Retrying.
E0210 23:33:34.624232    5968 start.go:135] Error starting the VM: Error creating the VM. Error with pre-create check: "Hyper-v commands have to be run as an Administrator". Retrying.
E0210 23:33:34.624232    5968 start.go:141] Error starting the VM:  Error creating the VM. Error with pre-create check: "Hyper-v commands have to be run as an Administrator"
Error creating the VM. Error with pre-create check: "Hyper-v commands have to be run as an Administrator"
Error creating the VM. Error with pre-create check: "Hyper-v commands have to be run as an Administrator"
```

Ya podían avisar en la doc :-( 

En fin, repito en un cmd abierto como administrador

```
c:\minishift>minishift start
Starting local OpenShift cluster using 'hyperv' hypervisor...
E0210 23:34:48.697286   12248 start.go:135] Error starting the VM: Error creating the VM. Error creating machine: Error in driver during machine creation: exit status 1. Retrying.
E0210 23:34:50.695140   12248 start.go:135] Error starting the VM: Error starting stopped host: exit status 1. Retrying.
E0210 23:34:52.668962   12248 start.go:135] Error starting the VM: Error starting stopped host: exit status 1. Retrying.
E0210 23:34:52.668962   12248 start.go:141] Error starting the VM:  Error creating the VM. Error creating machine: Error in driver during machine creation: exit status 1
Error starting stopped host: exit status 1
Error starting stopped host: exit status 1
```

Ya empezamos , grrr

El error no es que sea muy descriptivo, pero como en HyperV tengo imagenes de OpenShift completo (que no funcionan con solo 8 gigas en el portátil) sospecho que está intentando usar este nombre. Las renombro y lo vuelvo a intentar

```
c:\minishift>minishift start
Starting local OpenShift cluster using 'hyperv' hypervisor...
````

y esta vez parece que va y aparece una máquina llamada "minishift" en HyperV (si se llama distinto ¿por qué se quejaba?)

pues no, se queda en el start sin hacer nada y sin entro en la VM desde HyperV , está  arrancado pero sin hacer nada, con un consumo de RAM y CPU bajísimo

Viendo https://www.openshift.org/vm/ y 
https://github.com/minishift/minishift/issues/265


> "If you have already installed Docker Engine, Minishift will grab the VirtualSwitch for Docker Engine as it's network interface. This may or may not work for network access. If it does not work then you will not be able to complete the Minishift installation. To correct this:
> execute
> minishift stop
> make a new External VirtualSwitch in the Hyper-V console and assign it to the Minishift VM
> execute
> minishift start"

En mi caso no ha cogido el VirtualSwitch de Docker, ha pillado uno de otras pruebas, vamos el primero que ha encontrado.
Para generar el nuevo en el  "Administrator de HyperV"
Acciones->Administrador de conmutadores->Nuevo conmutador de red virtual->Externo->Crear conmutador virtual

Poner nombre

Tipo de conexion: Red Externa

Elegir el adaptador físico a usar (cable o wifi)... me temo que cuando este con cable y sin wifi esto no va a andar

Marcar "permitir que el sistema operativo de administración comparta este adaptador de red"

y con la máquina parada (minishift stop no funciona, hay que apagar desde HyperV), cambiar la configuración de la tarjeta de red y conectar la recién creada


Por fin

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



cambio otra vez el setpath (ya podian avisarlo al principio)

```
c:\minishift>more setpath.bat
set PATH=%PATH%;c:\minishift;C:\Users\pedroparra\.minishift\cache\oc\v1.4.1
```

tarda en arrancar, pero el ejemplo en node va

```
oc new-app https://github.com/openshift/nodejs-ex -l name=myapp
oc expose svc/nodejs-ex
```
y se puede acceder en http://nodejs-ex-myproject.{IP}.xip.io

la consola de openshift está en https://{IP}:8443/console  developer/developer