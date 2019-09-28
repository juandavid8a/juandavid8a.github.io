---
layout: post
title:  "Configurar FTP en Ubuntu Server"
description: Pasos para configurar FTP en Ubuntu Server
comments: true
category: Tutoriales
tags: Tutoriales Linux
---
Paso a paso para lograr para configurar FTP en Ubuntu Server

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
sudo apt-get update
sudo apt-get install vsftpd
sudo ufw status

//si ufw: command not found
sudo ufw allow 20/tcp
sudo ufw allow 21/tcp
sudo ufw allow 990/tcp
sudo ufw allow 40000:50000/tcp
```
