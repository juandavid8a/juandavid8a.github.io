---
layout: post
title:  "Cómo renombrar archivos en masa con Python"
description: Cómo renombrar archivos en masa con Python
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/6fsG3UX7Cfo
---
Paso a paso para renombrar archivos MP3 en masa y poder darle un orden aleatorio para reproducir en un radio vehicular

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

```C#
cont = 1
for i in file_list:
        print ("file: "+i)
        try:
                x = i.split(".")
                y = x[0].split("-")
                if len(y) == 1:
                    print(">>> no tiene separador \n")
                elif len(y) > 2:
                    print(">>> tiene más de un separador \n")
                else:
                    title = y[0].title().strip()
                    artist = y[1].title().strip()
                    
                current = EasyID3(i)
                newname = str(cont).zfill(3) + " " + title + " - " + artist + ".mp3"
                #print(newname)
                newname = current["title"][0] + ".mp3"
                #newname.replace(" ", "_")
                del current
                print ("renaming "+i+" to "+newname)
                os.rename(i, newname)
                cont = cont + 1
        except:
                print (">>> that file didn't work for some reason.")
```
