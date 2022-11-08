---
layout: post
title:  "Cómo instalar Raspbian en Raspberry Pi"
description: Cómo instalar Raspbian y otras configuraciones
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/EbKw0Dcaf6o
---
Listado de cosas por instalar en una Raspberry, otras aplicaciones y configuraciones.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

Raspberry Pi Image
```C#
https://www.raspberrypi.com/software/
```

Aplicación para saber la ip: 
```C#
https://www.advanced-ip-scanner.com/
```

Instalar escritorio remoto: 
```C#
sudo apt update
sudo apt upgrade
sudo apt install xrdp
```

Instalar docker
```C#
sudo apt update
sudo apt upgrade
sudo apt install raspberrypi-kernel raspberrypi-kernel-headers
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker pi
sudo reboot
docker version
```

Instalar Portainer
```C#
docker run -itd -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /docker/portainer:/data portainer/portainer-ce
localhost:9000
```
