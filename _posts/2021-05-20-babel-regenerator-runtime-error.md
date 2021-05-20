---
layout: post
title:  "Cómo solucionar regenerator runtime error nodejs"
description: Cómo solucionar ReferenceError: regeneratorRuntime is not defined en Heroku 
comments: true
category: Tutoriales
tags: Tutoriales
youtube: https://youtu.be/CDEaBtEeVwM
---
Un par de lineas necesarias para solucionar el error de node.js (ReferenceError: regeneratorRuntime is not defined) que se presenta cuando se usa Babel y despues de subir la aplicacion a Heroku.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
npm install -D  @babel/plugin-transform-runtime
npm install @babel/runtime
```
Agregar al archivo .babelrc
```PHP
"plugins": [
    ["@babel/plugin-transform-runtime",
      {
        "regenerator": true
      }
    ]
  ],
```
