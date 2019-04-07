---
layout: post
title:  "Solucionar problema CORS en Laravel 5"
description: Cómo solucionar el problema Access-Control-Allow-Origin - CORS en Laravel 5
comments: true
category: Tutoriales
tags: Tutoriales Laravel
youtube: https://youtu.be/KvldwGjCg5M
---
A continuación doy una solución rápida al problema Access-Control-Allow-Origin - CORS en Laravel 5 que sucede por razones de seguridad, los exploradores restringen las solicitudes HTTP de origen cruzado iniciadas dentro de un script.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
//Creamos un middleware
->header('Access-Control-Allow-Origin', '*’)
->header('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, OPTIONS’);

//ingresamos al Kernel
'cors' => \App\Http\Middleware\Cors::class,

//Agregamos a las rutas
Route::group(['middleware' => 'cors'], function(){
    //aqui van todas las rutas que necesitan CORS
}); 
```
