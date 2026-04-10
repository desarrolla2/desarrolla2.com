---
title: "Cuanto tarda en responder una petición HTTP"
date: 2012-10-10
tags: [linux, bash]
source: https://desarrolla2.com/post/cuanto-tarda-en-responder-una-peticion-http
---

# [Cuanto tarda en responder una petición HTTP](https://desarrolla2.com/post/cuanto-tarda-en-responder-una-peticion-http)

Existen principalmente dos comandos para obtener recursos a través de la linea de comandos: curl y wget, ambos tienen diferentes y utiles opciones, pero les falta una, que responda cuanto tardo en ejecutarse la petición.

Aqui está es la solución:

```
`(time wget -p --no-cache --delete-after http://desarrolla2.com/ -q ) 2>&1 | awk '/real/ {print $2}'`
```
