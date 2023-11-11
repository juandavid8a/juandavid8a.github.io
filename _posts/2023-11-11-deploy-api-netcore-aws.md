---
layout: post
title:  "Cómo hacer deploy de aplicación Net Core en AWS"
description: "Cómo hacer deploy de aplicación Net Core en AWS"
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/ScwqUMKhNm4
---
Código paso a paso para desplegar un proyecto API Net Core en AWS Elastic Beanstalk.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

1. Crear Aplicación Elastic Beanstalk
- Application Name | Enviroment | Domain | Platform Linux
- Create Service Role
- Create EC2 Intance Profile | IAM Role | WebTier | WorkerTier | MulticontainerDocker
- VPC | Public IP Address = Activated | Instance Subnets 
- EC2 Security Gropus = default
 
2. Configurar Visual Studio
- Crear Usuario | Crear Grupo | Crear Role = administrator
- Descargar e Instalar = AWS Toolkit for Visual Studio
- Crear perfil | Access Key | Secret Key
