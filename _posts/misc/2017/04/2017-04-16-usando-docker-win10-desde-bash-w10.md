---
title: "Bash y docker para w10"
categories:
  - misc
tags:
  - W10
  - Docker
  
excerpt: "Como usar el cliente Docker nativo de Linux desde Bash para W10 conectando con el docker de w10"
---

Mis notas sobre varias de las opciones:

## Instalar un cliente Docker en Ubuntu

Las [instrucciones](https://docs.docker.com/engine/installation/linux/ubuntu/) para montar Docker en Ubuntu son un auténtico coñazo y total en el Ubuntu que trae W10 no se puede correr Docker, ya que la emulación de Linux no es completa. Pero como indican en esta [respuesta](https://serverfault.com/questions/767994/can-you-run-docker-natively-on-the-new-windows-10-ubuntu-bash-userspace) de ServerFault, es posible usar el cliente en Linux atacando al motor de Docker de W10.

Como solo queremos el cliente, miramos en PowerShell o cmd la versión de Docker instalada en W10 y buscamos en este [enlace](https://github.com/docker/docker/releases) el enlace de descarga para W10 64 bits. Supongamos que es la 17.03-ce1, así que desde un bash de W10:

```
$ wget https://get.docker.com/builds/Linux/x86_64/docker-17.03.1-ce.tgz
$ tar -xvpf docker-17.03.1-ce.tgz docker/docker 
```

Así solo extraemos el cliente y pasamos del resto de binarios, como el *engine* que no podemos usarlo.

Ahora, para usar el *engine* del Docker en W10, lo comunicamos por TCP. Por ejemplo:


```
env DOCKER_HOST=tcp://0.0.0.0:2375 ./docker ps
```

Así ya podemos usar docker tanto desde una *shell* nativa de Windows (cmd o PowerShell) o desde el Ubuntu incorporado.

## Invocar al cliente nativo de W10


Como método alternativo, en *W10 Creator Edition* ya se permite que desde Bash se invoque a ejecutables nativos de Windows.

```
/mnt/c/Program\ Files/Docker/Docker/resources/bin/docker.exe ps
```

## Ejecutar desde MobyLinuxVM 

El docker de W10 realmente corre sobre una máquina virtual HyperV llamada MobyLinuxVM.

Estoy casi seguro que en versiones anteriores se podía entrar en una *shell* y ejecutar comandos, pero lo han debido capar en alguna actualización porque ya no consigo conectar desde el administrador de HyperV ni desde ssh o telnet. Supongo que estarían cansados de gente que entraba en la máquina, tocaba cosas y luego se quejaba de que no funcionaba Docker en Windows.