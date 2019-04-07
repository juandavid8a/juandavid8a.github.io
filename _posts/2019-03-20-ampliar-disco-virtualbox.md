---
layout: post
title:  "Resize Disk - VirtualBox - Linux - Windows"
description: Ampliar disco de virtualbox desde linux para windows
comments: true
category: Error
tags: Error VirtualBox
youtube: https://bit.ly/2wSo5iD
---

<p>Dos sencillos pasos para ampliar el disco de virtualbox desde Linux para Windows.</p>
<p>En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso: </p>

```
vboxmanage modifyhd "/home/ochoa/VirtualBox VMs/win10/win10.vdi" --resize 102400
```
y en windows:

1. Click derecho sobre el boton de inicio
2. Administración de discos
3. Click derecho extender volumen
