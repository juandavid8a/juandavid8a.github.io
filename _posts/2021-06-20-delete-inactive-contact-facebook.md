---
layout: post
title:  "Cómo listar contactos inactivos de facebook"
description: Cómo listar contactos inactivos de facebook para eliminarlos
comments: true
category: Tutoriales
tags: Tutoriales trucos
youtube: https://youtu.be/CDEaBtEeVwM
---
Codigo javascript que nos permite listar todos los contactos inactivos de facebook y despues poder borrarlos

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
let segundos = 0;

function hacerScrollUnMinuto() {
if(segundos < 10) {
segundos++
window.scrollBy(0, 1000)
setTimeout(hacerScrollUnMinuto, 1000)
}
}
function mostrarInactivos() {
let activos = document.querySelectorAll('.right[data-store*=is_deactivated\\"\\:false')
activos.forEach(p => p.parentElement.parentElement.parentElement.parentElement.remove())
window.scrollBy(0, 1000)
}
```
