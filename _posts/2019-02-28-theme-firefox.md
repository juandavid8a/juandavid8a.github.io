---
layout: post
title:  "Instalación Theme Arc-Dark Firefox"
description: Comandos de consola paso a paso para instalar el theme Arc-Dark en firefox para linux mint
comments: true
category: Tutoriales
tags: Tutoriales
youtube: https://bit.ly/2wSo5iD
---
A continuación describo los comandos de consola SSH para instalar en menos de 5 minutos el Theme Arc-Dark de Firefox para Linux Mint.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```
sudo rm -rf /usr/share/themes/{Arc,Arc-Darker,Arc-Dark}
rm -rf ~/.local/share/themes/{Arc,Arc-Darker,Arc-Dark}
rm -rf ~/.themes/{Arc,Arc-Darker,Arc-Dark}

sudo apt-get install autoconf automake pkg-config libgtk-3-dev
git clone https://github.com/horst3180/arc-theme --depth 1 && cd arc-theme
./autogen.sh --prefix=/usr
sudo make install
```
