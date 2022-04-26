---
layout: post
title:  "Cómo instalar HomeAsistant en Raspberry"
description: Cómo instalar Raspbian, Docker, Portainer, Homeasistant
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/EbKw0Dcaf6o
---
Paso a paso para instalar Homeasistant en una Raspberry, pero no queremos condenar toda una raspberry a ser solo un servidor de domotica, entonces vamos a instalar Raspbian, Docker, Portainer y por ultimo Homeasistant en un contenedor. De esta manera la Raspberry queda totalmente funcional con un sistema operativo corriendo el servidor de domotica y con la capacidad de correr otras aplicaciones o servidores en el mismo dispositivo.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```C#
sudo apt update
sudo apt upgrade
sudo apt install raspberrypi-kernel raspberrypi-kernel-headers
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker pi
sudo reboot
docker version

docker run -itd -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /docker/portainer:/data portainer/portainer-ce

```
