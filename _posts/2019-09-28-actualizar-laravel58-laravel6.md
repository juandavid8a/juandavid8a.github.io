---
layout: post
title:  "Actualizar Laravel 5.8 a Laravel 6"
description: Pasos para actualizar Laravel 5.8 a Laravel 6
comments: true
category: Tutoriales
tags: Laravel
youtube: https://youtu.be/Sp00ZqfBnM4
---
Paso a paso para lograr actualizar Laravel 5.8 a Laravel 6 

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
//Revisar version de PHP (debe tener PHP 7.2)
php -v

//En composer.json cambiar dependencias
"laravel/framework": "^6.0"
"php": "^7.2",
"phpunit/phpunit": "^8.0"

Ejecuta composer update para que se instalen las actualizaciones.
De igual forma, actualiza todos los paquetes de terceros consumidos por la aplicación, previa verificación de soporte para Laravel 6.0 en cada paquete.
```
