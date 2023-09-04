---
layout: post
title:  "Cómo hacer deployd de react.js en AWS"
description: "Cómo hacer deployd de react.js en AWS"
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/vItyn5jd-k8
---
Código paso a paso para desplegar un proyecto React.JS en AWS Elastic Beanstalk.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

1. Crear S3
- Block all public access
- Properties | Static website hosting | index.html
- Bucket policy | S3 | GetObject
- "Principal": "*"

2. Crear proyecto Reat.JS
- create-react-app reactapp
- npm run build
    
4. Hacer deploy
- login en aws
- https://aws.amazon.com/es/cli/
- aws --version
- aws configure
```C#
AWS Access Key ID [None]: AKIAURUY3VAMM35KPIJA
AWS Secret Access Key [None]: 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
Default region name [None]: us-west-2
Default output format [None]: json
```   
- aws s3 sync bulild/ s3://reactapp-bucket
