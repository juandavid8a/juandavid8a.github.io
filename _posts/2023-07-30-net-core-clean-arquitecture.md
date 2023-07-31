---
layout: post
title:  "Cómo integrar Clean Arquitecture a Net Core 7"
description: "Cómo integrar Clean Arquitecture a Net Core 7"
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/EbKw0Dcaf6o
---
Codigo paso a paso para integrar Clean Arquitecture a Net Core 7.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

1. Creamos una Blank Solution

2. Instalamos paquetes:
- Microsoft.Extensions.Options

3. Agregamos proyecto Class Library .API
- Controllers
- Responses
  
4. Agregamos proyecto Class Library .Core
- DTOs
- Entities
- Enumerations
- Interfaces
- Exceptions
- QueryFilters
- Services
  
5. Agregamos proyecto Class Library .Infrastructure
- Data
- Repositories
- Filters
- Mappings
- Validators

6. Reference
- .API = .Core, .Infrastructure
- .Infrastructure = .Core

7. Creamos MongoDbSettings Entity:
```C#
public required string ConnectionString { get; set; }        
public required string DatabaseName { get; set; }
```
