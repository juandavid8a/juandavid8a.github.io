---
layout: post
title:  "Solucionar problema CORS en Laravel 6"
description: Cómo solucionar el problema Access-Control-Allow-Origin - CORS en Laravel 6
comments: true
category: Tutoriales
tags: Tutoriales Laravel Error
youtube: https://youtu.be/KvldwGjCg5M
---
Este tutorial es la actualizacion de un post anterior que ya no funciona en versiones nuevas:
A continuación doy una solución rápida al problema Access-Control-Allow-Origin - CORS en Laravel 6 que sucede por razones de seguridad, los exploradores restringen las solicitudes HTTP de origen cruzado iniciadas dentro de un script.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
//Instalamos
composer require barryvdh/laravel-cors

//Agregamos esta linea al archivo config/app.php en el array de providers
Barryvdh\Cors\ServiceProvider::class,

//Agregamos esta linea al archivo app/Http/Kernel.php en "protected $middleware"
\Barryvdh\Cors\HandleCors::class,

//ejecutamos
php artisan vendor:publish --provider="Barryvdh\Cors\ServiceProvider"

//opcional agregar la linea en el archivo "App\Http\Middleware\VerifyCsrfToken"
protected $except = [
    'api/*'
];
```
