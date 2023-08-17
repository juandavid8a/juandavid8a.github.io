---
layout: post
title:  "Cómo enviar un topic a AWS IOT con Node.js"
description: "Cómo enviar un topic a AWS IOT con Node.js"
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/vItyn5jd-k8
---
Codigo paso a paso para conectar y enviar un topic a AWS IOT usando Node.js.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

1. Creamos un objeto en AWS y descargamos el código de ejemplo

2. Creamos un proyecto node
- npm init

3. Agregamos archivo:
- index.js
  
4. instalamos libreria 
- npm i aws-iot-device-sdk

5. Ingresamos el codigo node:

```C#
const awsIot = require('aws-iot-device-sdk');

const today = new Date();
const date = today.getFullYear() + '-' + (today.getMonth() + 1) + '-' + today.getDate();
const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
const dateTime = date + ' ' + time;
const topic = "topic"

const device = awsIot.device({
    clientId: 'clientId',
    host: 'host',
    port: 8883,
    keyPath: './private.key',
    certPath: './cert.pem',
    caPath: './CA.crt',
});

const IoTDevice = {
    serialNumber: "SN-D7F3C8947867",
    dateTime,
    activated: true,
    device: "MyRaspperry-01",
    type: "MySmartIoTDevice",
    payload: {}
}

const getSensorData = (cb) => getDummySensorData(cb);

const getDummySensorData = (cb) => {
    const temperatureData = { temp: '100°C', humidity: '52%' }
    return cb(temperatureData)
}

const sendData = (data) => {
    const telemetryData = {
        ...IoTDevice,
        payload: data
    }
    console.log(`STEP - Sending data to AWS  IoT Core'`, telemetryData)
    console.log(`---------------------------------------------------------------------------------`)
    return device.publish(topic, JSON.stringify(telemetryData))
}

device
    .on('connect', function () {
        console.log('STEP - Connecting to AWS  IoT Core');
        console.log(`---------------------------------------------------------------------------------`)
        setInterval(() => getSensorData(sendData), 3000)

    });

device
    .on('message', function (topic, payload) {
        console.log('message', topic, payload.toString());
    });

device
    .on('error', function (topic, payload) {
        console.log('Error:', topic, payload.toString());
    });
```
