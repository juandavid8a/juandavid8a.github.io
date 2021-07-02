---
layout: post
title:  "Cómo eliminar todas las tablas SQL"
description: Cómo eliminar todas las tablas de una base de datos SQL sin eliminar la base de datos
comments: true
category: Trucos
tags: Trucos
youtube: https://youtu.be/CDEaBtEeVwM
---
Código SQL para eliminar todas las tablas de una base de datos SQL sin eliminar la base de datos

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
DECLARE @sql NVARCHAR(max)=''

SELECT @sql += ' Drop table ' + QUOTENAME(TABLE_SCHEMA) + '.'+ QUOTENAME(TABLE_NAME) + '; '
FROM   INFORMATION_SCHEMA.TABLES
WHERE  TABLE_TYPE = 'BASE TABLE'

Exec Sp_executesql @sql
```
