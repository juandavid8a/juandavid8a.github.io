---
layout: post
title:  "Codigo C# para encriptar codigos de factura Cadena"
description: Cómo crear facilmente el codigo de encriptación y poder descargar facturas de Cadena
comments: true
category: Tutoriales
tags: Tutoriales c#
youtube: https://youtu.be/CDEaBtEeVwM
---
Este tutorial es un código muy simple para encriptar los numeros de facturas electronicas y poderlas bajar en PDF desde la plataforma de CADENA

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```PHP
private static readonly int tamanyoClave = 32;
private static readonly int tamanyoVector = 16;
// Define la palabra clave para el cifrado y
private static readonly string Clave = "Cadena_clave_para_cifrar";
private static readonly string Vector = "m2PCs0Ju9m1u3AIPkO3RUQ==";
// se convierte el vector y la key a bytes
public static byte[] Key = SHA256.Create().ComputeHash(Encoding.UTF8.GetBytes(Clave));
public static byte[] IV = Convert.FromBase64String(Vector);

Array.Resize(ref Key, tamanyoClave);
Array.Resize(ref IV, tamanyoVector);
Rijndael RijndaelAlg = Rijndael.Create();
MemoryStream memoryStream = new MemoryStream();
CryptoStream cryptoStream = new CryptoStream(memoryStream, RijndaelAlg.CreateEncryptor(Key, IV), CryptoStreamMode.Write);
byte[] txtPlanoBytes = UTF8Encoding.UTF8.GetBytes(txtPlano);
cryptoStream.Write(txtPlanoBytes, 0, txtPlanoBytes.Length);
cryptoStream.FlushFinalBlock();
byte[] cipherMessageBytes = memoryStream.ToArray();
memoryStream.Close();
cryptoStream.Close();
var numeroDeFacturaEncriptado = Convert.ToBase64String(cipherMessageBytes);
```
