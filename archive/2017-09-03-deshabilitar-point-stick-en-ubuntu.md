---
title: "Deshabilitar point stick en ubuntu"
date: 2017-09-03
tags: [ubuntu, linux]
source: https://desarrolla2.com/post/deshabilitar-point-stick-en-ubuntu
---

En mi último portatil tenía un problema como el point stick. ¿Que es eso? Es un pequeño botón que tienen en el centro del teclado algunos portátiles que casi nadie usa, y que hace las veces de ratón.

El problema era que detectava que habia movimiento, cuando no lo habia y hacia dificil trabajar con el. A pesar de que cambie el teclado de ese portatil el problema seguía existiendo, así que decidíi desabilitarlo, por si alguien le sucede algo parecido.

Añade lo siguiente al final de tu bash profile, y listo

```
`# Disable point stick
xinput -set-prop "AlpsPS/2 ALPS DualPoint Stick" "Device Enabled" 0`
```
