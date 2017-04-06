---
title: "Docker y la actualización a w10 creator edition"
categories:
  - misc
tags:
  - W10
  - Docker
  
excerpt: "Movidas con Docker al actualizar a w10 creator edition"
---

Pues he actualizado W10 a la *Creator Edition* y como era de esperar en una actualización tan grande, algo ha dejado de funcionar. En este caso, el afortunado ha sido Docker.

El error mostrado era

![Error arrancando Docker]({{ site.url }}{{ site.baseurl }}/assets/images/w10CreatorEdition.20170405/error-arrancando-docker.png)
{: .align-center}
{: .full}

Siendo el texto completo del mensaje de error el siguiente:

```
Unable to create: Se detuvo el comando en ejecución porque la variable de preferencia "ErrorActionPreference" o un parámetro común se han establecido en Stop: Hyper-V encontró un error al intentar acceder a un objeto en el equipo 'XXXXX' porque este no se encontró. Es posible que se haya eliminado el objeto. Asegúrese de que se esté ejecutando el servicio Administración de máquinas virtuales en el equipo.
en New-Switch, <sin archivo>: línea 131
en <ScriptBlock>, <sin archivo>: línea 381
   en Docker.Backend.HyperV.RunScript(String action, Dictionary`2 parameters)
   en Docker.Backend.ContainerEngine.Linux.Start(Settings settings)
   en Docker.Core.Pipe.NamedPipeServer.<>c__DisplayClass8_0.<Register>b__0(Object[] parameters)
   en Docker.Core.Pipe.NamedPipeServer.RunAction(String action, Object[] parameters)
```   

Los logs a los que hace referencia el cuadro de diálogo son los siguientes:

```
Version: 17.03.1-ce-win5 (10743)
Channel: stable
Sha1: b18e2a50ccf296bcd637b330c0ca9faaab9d790c
Started on: 2017/04/02 18:45:07.364
Resources: C:\Program Files\Docker\Docker\Resources
OS: Windows 10 Pro
Edition: Professional
Id: 1703
Build: 15063
BuildLabName: 15063.0.amd64fre.rs2_release.170317-1834
File: C:\Users\xxxxx\AppData\Local\Docker\log.txt
CommandLine: "C:\Program Files\Docker\Docker\Docker for Windows.exe" 
You can send feedback, including this log file, at https://github.com/docker/for-win/issues
[18:45:20.536][GUI            ][Info   ] Starting...
[18:45:46.380][Tracking       ][Info   ] Crash report and usage statistics are enabled
[18:45:46.692][SegmentApi     ][Info   ] Usage statistic: appLaunched
[19:05:55.457][NamedPipeClient][Info   ] Sending Version()...
[19:05:56.629][NamedPipeClient][Info   ] Received response for Version
[19:05:56.629][SegmentApi     ][Info   ] Usage statistic: heartbeat
[19:05:52.261][BackendServer  ][Info   ] Started
[19:05:56.428][NamedPipeServer][Info   ] Version()
[19:05:56.459][NamedPipeServer][Info   ] Version done in 00:00:00.
[19:06:07.740][Updater        ][Info   ] Checking for updates on channel stable...
[19:06:07.740][NamedPipeClient][Info   ] Sending Start(Docker.Core.Settings)...
[19:06:08.003][NamedPipeServer][Info   ] Start(Docker.Core.Settings)
[19:06:08.156][PowerMode      ][Info   ] Stop
[19:06:09.460][HyperV         ][Info   ] Stop
[19:06:10.478][Updater        ][Info   ] Local build 10743 is as good as the remote 10743 on channel stable
[19:06:12.871][PowerShell     ][Info   ] Run script with parameters: -Stop True...
[19:06:14.474][PowerShell     ][Info   ] Creating a Runspace Pool...
[19:06:51.393][PowerShell     ][Info   ] Runspace Pool created (Min=1, Max=2)
[19:07:00.765][HyperV         ][Info   ] Script started at 19:07:00.614
[19:07:37.472][HyperV         ][Info   ] Module loaded at 19:07:37.472
[19:07:39.847][HyperV         ][Info   ] VM MobyLinuxVM does not exist
[19:07:39.850][HyperV         ][Debug  ] [stop] took 00:01:29.3254033 to run
[19:07:39.851][OptimizeDisk   ][Info   ] Optimize
[19:07:39.876][PowerShell     ][Info   ] Run script...
[19:07:40.028][Moby           ][Info   ] Stop
[19:07:40.398][HyperVGuids    ][Info   ] Installing GUIDs...
[19:07:40.398][PowerMode      ][Info   ] Start
[19:07:40.399][HyperV         ][Info   ] Create
[19:07:40.400][PowerShell     ][Info   ] Run script with parameters: -Create True -VhdPathOverride  -SwitchSubnetAddress 10.0.75.0 -SwitchSubnetMaskSize 24 -CPUs 2 -Memory 2048 -IsoFile C:\Program Files\Docker\Docker\Resources\mobylinux.iso...
[19:07:40.401][HyperVGuids    ][Info   ] GUIDs installed
[19:07:40.567][HyperV         ][Info   ] Script started at 19:07:40.567
[19:07:40.582][Firewall       ][Info   ] Removing all existing rules...
[19:07:40.604][HyperV         ][Info   ] Module loaded at 19:07:40.603
[19:07:41.291][Firewall       ][Info   ] All existing rules are removed.
[19:07:41.292][Firewall       ][Info   ] Opening ports for C:\Program Files\Docker\Docker\Resources\com.docker.proxy.exe...
[19:07:41.341][Firewall       ][Info   ] Opening ports for SMB...
[19:07:41.358][Firewall       ][Info   ] Ports are opened
[19:07:42.892][HyperV         ][Info   ] Creating Switch: DockerNAT...
[19:07:44.035][Linux          ][Error  ] Failed to start: Unable to create: Se detuvo el comando en ejecución porque la variable de preferencia "ErrorActionPreference" o un parámetro común se han establecido en Stop: Hyper-V encontró un error al intentar acceder a un objeto en el equipo 'XXXXX' porque este no se encontró. Es posible que se haya eliminado el objeto. Asegúrese de que se esté ejecutando el servicio Administración de máquinas virtuales en el equipo.
en New-Switch, <sin archivo>: línea 131
en <ScriptBlock>, <sin archivo>: línea 381. Let's retry
[19:07:44.037][PowerShell     ][Info   ] Run script...
[19:08:05.063][HyperV         ][Info   ] Starting Hyper-V Virtual Machine Management service
[19:08:05.675][HyperV         ][Info   ] Hyper-V is running
[19:08:05.675][PowerMode      ][Info   ] Stop
[19:08:05.675][HyperV         ][Info   ] Stop
[19:08:05.676][PowerShell     ][Info   ] Run script with parameters: -Stop True...
[19:08:05.686][HyperV         ][Info   ] Script started at 19:08:05.685
[19:08:05.698][HyperV         ][Info   ] Module loaded at 19:08:05.698
[19:08:05.766][HyperV         ][Info   ] VM MobyLinuxVM does not exist
[19:08:05.766][HyperV         ][Debug  ] [stop] took 00:00:00.0905198 to run
[19:08:05.766][OptimizeDisk   ][Info   ] Optimize
[19:08:05.766][PowerShell     ][Info   ] Run script...
[19:08:05.815][Moby           ][Info   ] Stop
[19:08:05.823][HyperV         ][Info   ] Destroy
[19:08:05.824][PowerShell     ][Info   ] Run script with parameters: -Destroy True -KeepVolume True...
[19:08:05.860][HyperV         ][Info   ] Script started at 19:08:05.860
[19:08:05.872][HyperV         ][Info   ] Module loaded at 19:08:05.872
[19:08:05.946][HyperV         ][Info   ] VM MobyLinuxVM does not exist
[19:08:05.950][HyperV         ][Info   ] Destroying Switch DockerNAT...
[19:08:06.051][HyperV         ][Info   ] Removing VM MobyLinuxVM...
[19:08:06.107][HyperV         ][Debug  ] [destroy] took 00:00:00.2830507 to run
[19:08:06.108][Firewall       ][Info   ] Closing ports...
[19:08:06.108][Firewall       ][Info   ] Removing all existing rules...
[19:08:06.217][Firewall       ][Info   ] All existing rules are removed.
[19:08:06.217][Firewall       ][Info   ] Ports are closed
[19:08:06.217][HyperVGuids    ][Info   ] Removing GUIDs...
[19:08:06.250][HyperVGuids    ][Info   ] GUIDs removed
[19:08:06.250][HyperV         ][Info   ] Create
[19:08:06.251][PowerShell     ][Info   ] Run script with parameters: -Create True -VhdPathOverride  -SwitchSubnetAddress 10.0.75.0 -SwitchSubnetMaskSize 24 -CPUs 2 -Memory 2048 -IsoFile C:\Program Files\Docker\Docker\Resources\mobylinux.iso...
[19:08:06.261][HyperV         ][Info   ] Script started at 19:08:06.261
[19:08:06.280][HyperV         ][Info   ] Module loaded at 19:08:06.279
[19:08:06.428][HyperV         ][Info   ] Creating Switch: DockerNAT...
[19:08:06.560][PowerMode      ][Info   ] Stop
[19:08:06.560][HyperV         ][Info   ] Stop
[19:08:06.560][PowerShell     ][Info   ] Run script with parameters: -Stop True...
[19:08:06.571][HyperV         ][Info   ] Script started at 19:08:06.571
[19:08:06.583][HyperV         ][Info   ] Module loaded at 19:08:06.583
[19:08:06.629][HyperV         ][Info   ] VM MobyLinuxVM does not exist
[19:08:06.629][HyperV         ][Debug  ] [stop] took 00:00:00.0690285 to run
[19:08:06.629][OptimizeDisk   ][Info   ] Optimize
[19:08:06.629][PowerShell     ][Info   ] Run script...
[19:08:06.673][Moby           ][Info   ] Stop
[19:08:06.679][HyperV         ][Info   ] Destroy
[19:08:06.683][PowerShell     ][Info   ] Run script with parameters: -Destroy True -KeepVolume True...
[19:08:06.695][HyperV         ][Info   ] Script started at 19:08:06.695
[19:08:06.711][HyperV         ][Info   ] Module loaded at 19:08:06.710
[19:08:06.777][HyperV         ][Info   ] VM MobyLinuxVM does not exist
[19:08:06.778][HyperV         ][Info   ] Destroying Switch DockerNAT...
[19:08:06.867][HyperV         ][Info   ] Removing VM MobyLinuxVM...
[19:08:06.918][HyperV         ][Debug  ] [destroy] took 00:00:00.2388384 to run
[19:08:06.918][Firewall       ][Info   ] Closing ports...
[19:08:06.918][Firewall       ][Info   ] Removing all existing rules...
[19:08:06.947][Firewall       ][Info   ] All existing rules are removed.
[19:08:06.948][Firewall       ][Info   ] Ports are closed
[19:08:06.948][HyperVGuids    ][Info   ] Removing GUIDs...
[19:08:06.948][HyperVGuids    ][Info   ] GUIDs removed
[19:08:06.951][NamedPipeServer][Error  ] Unable to execute Start: Unable to create: Se detuvo el comando en ejecución porque la variable de preferencia "ErrorActionPreference" o un parámetro común se han establecido en Stop: Hyper-V encontró un error al intentar acceder a un objeto en el equipo 'XXXXX' porque este no se encontró. Es posible que se haya eliminado el objeto. Asegúrese de que se esté ejecutando el servicio Administración de máquinas virtuales en el equipo.
en New-Switch, <sin archivo>: línea 131
en <ScriptBlock>, <sin archivo>: línea 381    en Docker.Backend.HyperV.RunScript(String action, Dictionary`2 parameters)
   en Docker.Backend.ContainerEngine.Linux.Start(Settings settings)
   en Docker.Core.Pipe.NamedPipeServer.<>c__DisplayClass8_0.<Register>b__0(Object[] parameters)
   en Docker.Core.Pipe.NamedPipeServer.RunAction(String action, Object[] parameters)
[19:08:08.947][NamedPipeClient][Error  ] Unable to send Start: Unable to create: Se detuvo el comando en ejecución porque la variable de preferencia "ErrorActionPreference" o un parámetro común se han establecido en Stop: Hyper-V encontró un error al intentar acceder a un objeto en el equipo 'XXXXX' porque este no se encontró. Es posible que se haya eliminado el objeto. Asegúrese de que se esté ejecutando el servicio Administración de máquinas virtuales en el equipo.
en New-Switch, <sin archivo>: línea 131
en <ScriptBlock>, <sin archivo>: línea 381
[19:08:08.949][Notifications  ][Error  ] Unable to create: Se detuvo el comando en ejecución porque la variable de preferencia "ErrorActionPreference" o un parámetro común se han establecido en Stop: Hyper-V encontró un error al intentar acceder a un objeto en el equipo 'XXXXX' porque este no se encontró. Es posible que se haya eliminado el objeto. Asegúrese de que se esté ejecutando el servicio Administración de máquinas virtuales en el equipo.
en New-Switch, <sin archivo>: línea 131
en <ScriptBlock>, <sin archivo>: línea 381
[19:09:00.059][ErrorReportWindow][Info   ] Open logs
```

La parte relevante parece esta:

```
[...]Hyper-V encontró un error al intentar acceder a un objeto en el equipo 'XXXXX' porque este no se encontró. Es posible que se haya eliminado el objeto. Asegúrese de que se esté ejecutando el servicio Administración de máquinas virtuales en el equipo.[...]
```

¿cómo? ¿qué la actualización se ha cargado la máquina virtual que usa Docker? Arranco el "Administrador de Hyper-V", para comprobarlo y la máquina sigue allí, aunque desactivada

![MobyLinux VM desactivada]({{ site.url }}{{ site.baseurl }}/assets/images/w10CreatorEdition.20170405/MobyLinuxVM.desactivada.png)
{: .align-center}
{: .full}

¿será cómo dice el *log* que el servicio de administración de máquinas virtuales no está arrancado? Pues no, parece arrancado

![MobyLinux VM desactivada]({{ site.url }}{{ site.baseurl }}/assets/images/w10CreatorEdition.20170405/Administrador-HyperV.png)
{: .align-center}
{: .full}

Y como siempre en estos casos misteriosos, después de ver que el servicio estaba arrancado, al volver a reintentar, todo funcionaba a la perfección. ¿será que al arrancar el administrador de Hyper-V se inició el servicio?