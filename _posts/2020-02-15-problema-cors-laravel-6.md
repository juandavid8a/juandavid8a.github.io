---
layout: post
title:  "Solucionar problema CORS en Laravel 6 | 2020"
description: Cómo solucionar el problema Access-Control-Allow-Origin - CORS en Laravel 6
comments: true
category: Tutoriales
tags: Tutoriales Laravel Error
youtube: https://youtu.be/CDEaBtEeVwM
---
Este tutorial es la actualización de un post anterior que ya no funciona ya que la libreria barryvdh/laravel-cors no esta disponible
A continuación doy una solución rápida al problema Access-Control-Allow-Origin - CORS en Laravel 6 que sucede por razones de seguridad, los exploradores restringen las solicitudes HTTP de origen cruzado iniciadas dentro de un script.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
//Instalamos
composer require fruitcake/laravel-cors

//Agregamos esta linea al archivo app/Http/Kernel.php
//en "protected $middleware"
\Fruitcake\Cors\HandleCors::class,

//Creamos arvhivo de conficuración
php artisan vendor:publish --tag="cors"

//Abrimos el archivo de configuracion config/cors.php y agregamos el Path que queremos darle acceso de origenes
'paths' => ['api/*'],
```
