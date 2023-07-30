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

Creamos una Blank Solution
Agregamos proyecto Class Library .API
  Controllers
  Responses
  
Agregamos proyecto Class Library .Core
  DTOs
  Entities
  Enumerations
  Interfaces
  Exceptions
  QueryFilters
  Services
  
Agregamos proyecto Class Library .Infrastructure
  Data
  Repositories
  Filters
  Mappings
  Validators

Reference
  .API = .Core, .Infrastructure
  .Infrastructure = .Core
