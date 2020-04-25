---
layout: post
title:  "Cómo importar un excel o CSV a firebase firestore"
description: Cómo importar un excel o CSV a firebase firestore
comments: true
category: Tutoriales
tags: Tutoriales Firebase
youtube: https://youtu.be/ut9QZQX2_as
---
Paso a paso para importar un archivo excel o CSV a una base de datos firestore de firebase 

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
//Ingresamos a consola
https://console.cloud.google.com

//ejecutamos
gcloud config set project IDPROJECT
mkdir csv
npm init
npm install @google-cloud/firestore
npm install csv-parse
touch csvExport.js
node csvExport.js archivo.csv
```
