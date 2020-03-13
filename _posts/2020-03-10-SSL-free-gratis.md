---
layout: post
title:  "Cómo instalar un certificado SSL gratis en Ubuntu Server"
description: Cómo instalar un certificado SSL gratis en Ubuntu Server
comments: true
category: Tutoriales
tags: Tutoriales Linux
youtube: https://youtu.be/CDEaBtEeVwM
---
Paso a paso para instalar un certificado SSL gratis en servidor web ubuntu

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
//instalamos Certbot
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot python-certbot-apache -y
```
