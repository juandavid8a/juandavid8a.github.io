---
layout: post
title:  "WordPress Memory Exhausted Error | WP Engine"
description: Cómo arreglar el error WordPress Memory Exhausted Error – Increase PHP Memory
comments: true
category: Tutoriales
tags: Trucos Error
youtube: https://youtu.be/CDEaBtEeVwM
---
Un par de lineas necesarias para arreglar en WordPress el error Memory Exhausted Error – Increase PHP Memory y en este caso lo haremos en la plataforma WP Engine

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

Debemos buscar el archivo "wp-config.php" y agregar las siguientes lineas

```PHP
define('WP_MEMORY_LIMIT', '256M');
define( 'WP_MAX_MEMORY_LIMIT', '512M' );
```
