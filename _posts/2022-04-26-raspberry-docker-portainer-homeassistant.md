---
layout: post
title:  "Cómo instalar HomeAssistant en Raspberry Pi"
description: Cómo instalar Raspbian, Docker, Portainer, Homeassistant
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/EbKw0Dcaf6o
---
Paso a paso para instalar Homeassistant en una Raspberry, pero no queremos condenar toda una raspberry a ser solo un servidor de domotica, entonces vamos a instalar Raspbian, Docker, Portainer y por ultimo Homeassistant en un contenedor. De esta manera la Raspberry queda totalmente funcional con un sistema operativo corriendo el servidor de domotica y con la capacidad de correr otras aplicaciones o servidores en el mismo dispositivo.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```C#
https://www.advanced-ip-scanner.com/

sudo apt update
sudo apt upgrade
sudo apt install raspberrypi-kernel raspberrypi-kernel-headers
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker pi
sudo reboot
docker version

docker run -itd -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /docker/portainer:/data portainer/portainer-ce
192.168.1.1:9000

sudo apt-get install jq
sudo apt-get install wget
sudo apt-get install curl
sudo apt-get install udisks2
sudo apt-get install libglib2.0-bin
sudo apt-get install network-manager
sudo apt-get install dbus

https://github.com/home-assistant/os-agent/releases/tag/1.2.2
wget https://github.com/home-assistant/os-agent/releases/download/1.2.2/os-agent_1.2.2_linux_armv7.deb
dpkg -i os-agent_1.2.2_linux_armv7.deb
wget https://github.com/home-assistant/supervised-installer/releases/latest/download/homeassistant-supervised.deb
dpkg -i homeassistant-supervised.deb
192.168.1.1:8123
```
