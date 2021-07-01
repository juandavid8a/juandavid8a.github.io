---
layout: post
title:  "Cómo instalar service entityUser en NetCore 5"
description: Cómo instalar service entityUser en NetCore 5
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/CDEaBtEeVwM
---
Paso a paso para instalar y usar el servicio entityUser que trae por defecto .Net Core 5

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

1. Crear la carpeta Models
2. Crear el modelo ApplicationUser
```PHP
    public class ApplicationUser : IdentityUser
    {
    }
```

3. Crear un modelo de prueba
4. Crear un controlador de prueba para el modelo de prueba
5. Renombrar el DbContext por ApplicationDbContext
6. Actualizar el DbContext
```PHP
    : IdentityDbContext<ApplicationUser>
```

7. Actualizar el ConnectionString
8. Actualizar dependencias
9. Crear migracion
10. Actualizar base de datos
