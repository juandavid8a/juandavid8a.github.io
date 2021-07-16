---
layout: post
title:  "Solucionar problema CORS en .Net Core 5"
description: Cómo Solucionar problema CORS en .Net Core 5
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/CDEaBtEeVwM
---
Un par de lineas para solucionar el problema de CORS en API .Net Core 5

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

- Poner en el archivo startup en la seccion de servicios
```C#
services.AddCors(options =>
{
    options.AddDefaultPolicy(builder => { 
        builder
        .AllowAnyOrigin()
        .AllowAnyHeader()
        .AllowAnyMethod(); 
    });
});
```

- Poner en el archivo startup en la seccion de aplicaciones
```C#
app.UseCors();
```
