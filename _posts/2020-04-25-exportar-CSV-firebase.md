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

```PHP
const {readFile}  = require('fs').promises;
const {promisify} = require('util');
const parse       = promisify(require('csv-parse'));
const {Firestore} = require('@google-cloud/firestore');

if (process.argv.length < 3) {
  console.error('Please include a path to a csv file');
  process.exit(1);
}


const db = new Firestore();

function writeToFirestore(records) {
  const batchCommits = [];
  let batch = db.batch();
  records.forEach((record, i) => {
    var docRef = db.collection('rainfall').doc(record.SUBDIVISION);
    batch.set(docRef, record);
    if ((i + 1) % 500 === 0) {
      console.log(`Writing record ${i + 1}`);
      batchCommits.push(batch.commit());
      batch = db.batch();
    }
  });
  batchCommits.push(batch.commit());
  return Promise.all(batchCommits);
}

async function importCsv(csvFileName) {
  const fileContents = await readFile(csvFileName, 'utf8');
  const records = await parse(fileContents, { columns: true });
  try {
    await writeToFirestore(records);
  }
  catch (e) {
    console.error(e);
    process.exit(1);
  }
  console.log(`Wrote ${records.length} records`);
}

importCsv(process.argv[2]).catch(e => console.error(e));
```
