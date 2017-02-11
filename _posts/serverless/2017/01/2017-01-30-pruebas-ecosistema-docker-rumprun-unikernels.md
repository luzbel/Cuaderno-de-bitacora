---
title: "Ecosistema docker para trabajar con rumprun unikernels"
modified: 2017-02-11
categories:
  - Serverless
tags:
  - Unikernels
  - RumpKernel 
  
excerpt: "Ecosistema docker para trabajar con rumprun unikernes"
header:
  teaser: /assets/images/Alfredo_Landa.teaser.jpg
---

Pruebo los dockers disponibles en [GitHub](https://github.com/mato/rumprun-docker-builds) para trabajar con rumprun unikernels:

```
C:> docker run -ti mato/rumprun-toolchain-hw-x86_64
$ x86_64-rumprun-netbsd-gcc -o hello hello.c
$ rumprun-bake hw_virtio hello.bin hello
$ rumprun iso hello 
C:> docker cp 02405a71bab9:/build/rumprun-hello.iso .  
``` 

Al intentar ejecutar esa ISO  en una HyperV generación 2 da error, al no ser una ISO compatible con UEFI

Al intentar ejecutar esa ISO  en una HyperV generación 1 da error, el GRUB se queja de que no hay cabecera multiboot

Si genero la imagen a partir del binario y no del ejecutable

```
$ rumprun iso hello.bin
C:> docker cp 02405a71bab9:/build/rumprun-hello.bin.iso .  
```

Al intentar ejecutar esa ISO  en una HyperV generación 1 da error, el unikernel empieza a arrancar pero se queja de:

```
...
mounted tmpfs on /tmp
rumprun: failed to mount rootfs on image
```

pruebo a montar un fs embebido con cookfs
```
compile unikernel
mkdir rootfs ; vi rootfs/json.cfg
x86_64-rumprun-netbsd-cookfs s0.fs rootfs
rumprun-bake -m "add s0.fs" hw_generic s0.bin unikernel
rumprun iso s0.bin
qemu-system-x86_64 -curses -s -cdrom rumprun-s0.bin.iso
```

Falla con "mkdir /rootfs failed: File exists", porque el / ya debe existir, uhm...

Debería montarse como /, no tengo mucha esperanza de que sirva pero pruebo el flag -s de cookfs
```
compile unikernel
mkdir rootfs ; vi rootfs/json.cfg
x86_64-rumprun-netbsd-cookfs -s 1 s1.fs rootfs
rumprun-bake -m "add s1.fs" hw_generic s1.bin unikernel
rumprun iso s1.bin
qemu-system-x86_64 -curses -s -cdrom rumprun-s1.bin.iso
```

Falla con "failed to mount rootfs from image". Supongo que porque al compilar, por defecto, no se [incluyen los controladores para PCI](https://github.com/rumpkernel/rumprun/issues/24)

pruebo node en vez de mi helloworld, ya que 

```
$ mkdir testnode
$ cd testnode
$ git clone https://github.com/rumpkernel/rumprun-packages.git
$ cd rumprun-package
$ cp config.mk.dist config.mk
$ vi config.mk
$ more config.mk
#
# rumprun-packages "configuration"
#

# Set to the name of the rumprun compiler toolchain you want to use for
# building packages. (eg. x86_64-rumprun-netbsd, i486-rumprun-netbsdelf).
RUMPRUN_TOOLCHAIN_TUPLE=x86_64-rumprun-netbsd

# Select ssl package (another option is openssl)
#RUMPRUN_SSL= libressl

$ cd nodejs
$ make
```

(falla por no tener wget)

```
$ sudo su -
# apt-get install wget
# apt-get install vim
$ make
``` 

(falla por no tener python, manda egg que para un lenguaje haga falta otro)

```
$ sudo su -
# apt-get install python
$ make 
```

Como no consigo que vaya con HyperV, probaré con xen y qemu

```
$ sudo su -
# apt-get install xen-hypervisor-amd64
```

Pero parece que para que funcione xen, hay que reiniciar y cargar en grub otro kernel para activar cosas
un "# xl list" da error tal cual

```
$ sudo su -
# apt-get install qemu
```

no hay ejecutable qemu , es qemu-system-i386 o qemu-system-x86_64
lo que hace "rumprun qemu" es lanzar qemu-system-x86_64 con la opcion kernel para ejecutar uno (ver http://wiki.qemu.org/download/qemu-doc.html#direct_005flinux_005fboot)

hay que añadir el "-curses" a qemu con "-g" porque al estar en docker no tengo interfaz gráfica y las SDL de qemu no van

```
$ mkdir testqemu
$ cp ../hello.c .
$ x86_64-rumprun-netbsd-gcc -o hello hello.c
$ rumprun-bake hw_generic hello.hw_generic.bin hello
$ rumprun-bake hw_virtio hello.hw_virtio.bin hello
$ rumprun qemu -i -g "-curses" hello.hw_virtio.bin
$ rumprun qemu -i -g "-curses" hello.hw_generic.bin
$ qemu-system-x86_64 -kernel hello.hw_generic.bin -curses
```

arranca, aunque da este error "rumprun: could not find start of json.  no config?"

```
$ qemu-system-x86_64 -kernel hello.hw_virtio.bin -curses
```

arranca, aunque da este error "rumprun: could not find start of json.  no config?"

supongo que por faltar el parametro "append" del direct boot de kvm , que pasará parámetros al kernel


#### notas sueltas

así es como el [ejemplo de node](https://github.com/rumpkernel/rumprun-packages/blob/403208f7bd60f4e8219d5e23e1bb918076388069/nodejs/README.md) añade un filesystem embebido al unikernel

```
rumprun-bake -m "add express-4.13.3.fs" hw_generic ../build-4.3.0/out/Release/node-express-4.13.3.bin ../build-4.3.0/out/Release/node-default
```

y este es el aspecto de algunos ficheros de configuración

```
 {,
        "cmdline": "hello.bin",
        "hostname": "rumprun-hello.bin.ec2dir",
},

 {,        "cmdline": "hello.bin",     "hostname": "rumprun-hello.bin.ec2dir", },

{        "cmdline": "hello.bin",     "hostname": "rumprun-hello.bin.ec2dir" }


{        "cmdline": "kk.bin",     "hostname": "rumprun-kk.bin.iso" }
```

Y esto es como se hace en enlazado final para generar el unikernel. Uhm, no me queda claro cuando enlaza el objeto con el sistema de archivos embebido generado por cookfs.

```
# Final link using cc to produce the unikernel image.
${runcmd} ${RUMPBAKE_BACKINGCC} ${RUMPBAKE_CFLAGS}			\
    -specs=${RUMPBAKE_TOOLDIR}/share/specs-bake-${RUMPBAKE_TUPLE}-${PLATFORM}	\
    -o ${OUTPUT} ${allobjs} ${TMPDIR}/cfg.c				\
    -Wl,--whole-archive ${LIBS} || exit 1

exit 0
```

#### Ideas locas

creo que tocando rumprun , en la parte que genera el grub.cfg y cambiando el _RUMPRUN_ROOTFSCFG=
por un json con una config minima , podría arrancar

Aasi al llegar a /lib/librumprun_base/config.c
rumprun_config_path se piensa que no se le ha pasado que se lea el json.cfg de un dispositivo

entonces rumprun_config parsea la cmdline como si fuese el propio json.cfg

si va, en vez de tocar rumprun , habría que generar un nuevo target (igual que ec2 o iso que podria llamarse diskless o algo asi)

con multiboot y el json en grub.cfg da error parseo del json

Es decir, antes el grub.cfg tenía esta pinta

```
set timeout=0
menuentry "rumpkernel" {
	multiboot /boot/unikernel.bin _RUMPRUN_ROOTFSCFG=/json.cfg
}
```

Y ahora esta, para que se pase directamente al kernel por línea de comando la configuración

```
set timeout=0
menuentry "rumpkernel" {
	multiboot {        "cmdline": "hello.bin",     "hostname": "rumprun-hello.bin.iso" }
}
```

Pero falla, porque "{" es un caracter reservado para grub. 

Consigo escaparlo:
```
set timeout=0
menuentry "rumpkernel" {
	multiboot \{        "cmdline": "hello.bin",     "hostname": "rumprun-hello.bin.iso" \}
}
```

Pero al kernel le llegan los objetos sin entrecomillar y no le sienta bien al cutre parseador de JSON que usa el kernel.

En ec2 el iso es distinto y no va con multiboot

```
default 0
timeout 1
title rumprun-.._hello.bin.ec2dir
  root (hd0)
  kernel /boot/hello.bin _RUMPRUN_ROOTFSCFG=/json.cfg
```  
no avanzo  por esa línea

#### Buscando alternativas

pero ... cuando falla al parsear el json.cfg da un warning, pero el ejecutable funciona sin problema
  
asi, ¿y si cambio rumprun  para que en mi configuracion no le pase ni "_RUMPRUN_ROOTFSCFG=" (que obliga a intentar montar el disco y no hay drivers pci para baremetal integrados)
 ni pasando un json, que averigua que formato espera o que es lo que le llega en la cmdline
 
pues al menos arranca, habrá que ver que pasa luego para tener red u otras cosas interesantes
 
y arranca tanto en qemu (sin rumprun) con

```
$ qemu-system-x86_64 -curses -cdrom rumprun-kk.bin.iso
```

como en HyperV
 
 
por cierto que el append con los parametros que pasa rumprun a qemu es

```
qemu-system-x86_64 -net none -no-kvm -m 64 -curses -kernel hello.hw_generic.bin -append  " { \"cmdline\": \"hello.hw_generic.bin\" } "
``` 


#### Vamos a investigar 

instalo

```
$ sudo su -
# apt-get install gdb
# apt-get install gdb-multiarch
```

para depurar es un lio, porque el gdb al analizar el unikernel.bin ve que el código es de 64 bits
pero en el arranque se pasa por 16 y 32 bits y se lia

si haces simplmente gdb kk.bin y te enganchas al target remote:1234 al llegar al breakpoint salta
el error remote 'g' packet reply is to long

los pasos son

* lanzar qemu en una sesion shell , importante no pasar el -S , solo el -s
* sesion1 $ qemu-system-x86_64 -curses -s -cdrom rumprun-kk.bin.iso

o desde rumprun sin iso (pero sin iso no tenemos problema de ver la cmdline que llega por multiboot)

* sesion1 $ rumprun qemu -p -D 1234 -i -g "-curses" kk.bin

abrir un gdb temporal en otra sesion
lo explican (aunque mal y no queda claro ) en https://github.com/rumpkernel/wiki/wiki/Howto:-Debugging-Rumprun-with-gdb

```
sesion2 $ gdb-mu(gdb) target remote:1234
Remote debugging using :1234
0x0000000000000000 in ?? ()
(gdb) break x86_boot
Breakpoint 2 at 0x102a10: file arch/x86/boot.c, line 37.
(gdb) c
Continuing.
Remote 'g' packet reply is too long: 1100018000000000d403010000000000800000c0000000000000000000000000000000000000000000000100000000000000000000000000081010000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000102a1000000000004600200008000000180000001800000018000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000007f0300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000801f0000
(gdb) disconnect
Ending remote debugging.
(gdb) quit
```

cuando damos al c (continue) en gdb , en la sesion 1, modificamos nuestros parametros del kernel
y damos en grub al arrancar
cuando llega a x86_boot es cuando peta el gdb y nos desconectamos y nos salimos,
pero el qemu queda parado en el breakpoint

ahora nos vamos de nuevo a la sesion2 y arrancamos otra vez gdb

```
$ gdb-multiarch kk.bin
GNU gdb (Debian 7.7.1+dfsg-5) 7.7.1
Copyright (C) 2014 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from kk.bin...done.
(gdb) target remote:1234
Remote debugging using :1234
x86_boot (mbi=0x10000) at arch/x86/boot.c:37
37      arch/x86/boot.c: No such file or directory.
(gdb) break rumprun_config
Breakpoint 1 at 0x318e20: file /build/rumprun/lib/librumprun_base/config.c, line 747.
(gdb) c
Continuing.

Breakpoint 1, rumprun_config (cmdline=cmdline@entry=0x48f480 <multiboot_cmdline> "{ \\\"cmdline\\\": \\\"kk.bin\\\" }")
``` 

aqui podemos hacer next y ver los valores de tokens,
el primero esta bien, objeto, pero los otros 2 salen como primitivas y no como strings

```
(gdb) print tokens[0]
$1 = {type = JSMN_OBJECT, start = 0, end = 27, size = 1}
(gdb) print tokens[1]
$2 = {type = JSMN_PRIMITIVE, start = 2, end = 13, size = 1}
(gdb) print tokens[2]
$3 = {type = JSMN_PRIMITIVE, start = 15, end = 25, size = 0}
```

cuando no inyecto la conf en grub y la lee del filesystem se ve así

```
Breakpoint 1, rumprun_config (
    cmdline=cmdline@entry=0x48f480 <multiboot_cmdline> "kk.bin  {, \n\t\"cmdline\": \"kk.bin\", \n}, ")
```

bueno, al final no se como escapar en grub, así que usamos el protocolo multiboot y sus modulos

en vez de tener un grub.cfg asi

```
set timeout=0
menuentry "rumpkernel" {
	multiboot /boot/hello.cookfs.bin _RUMPRUN_ROOTFSCFG=/json.cfg
}
```

pasamos a generar
```
set timeout=0
menuentry "rumpkernel" {
	multiboot /boot/hello.hw_generic.bin 
	module /json.cfg cmdline  
}
```

hacemos un fork , clonamos el repo 
y para subir el cambio a rumprun configuramos git

```
build@02405a71bab9:~/rumpgit/rumprun$ git config --global user.email micuenta@decorreo
build@02405a71bab9:~/rumpgit/rumprun$ git config --global user.name miUsuario
```

Y hacemos un [PR](https://github.com/rumpkernel/rumprun/pull/90) a ver si les parece bien y lo aceptan