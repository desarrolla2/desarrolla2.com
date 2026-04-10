---
title: "¿Cuanto tiempo llevo sin formatear mi ordenador?"
date: 2017-08-15
tags: [ubuntu, linux, bash]
source: https://desarrolla2.com/post/cuanto-tiempo-llevo-sin-formatear-mi-ordenador
---

Cuando un sistema operativo tiene bastante tiempo funcionando empiezan a fallar las cosas. El navegador se cuelga, el sistema informa de errores con frecuencia, a veces se requiere un reinicio ... etc.

Es normal. Yo recomiendo formatear los equipos cada año o dos años. Vamos instalando cosas a medida que las necesitamos, muchas veces instalamos cosas por simple curiosidad, y muchas veces olvidamos desinstalarlas cuando ya no vamos a volver a usarlas.

Cuando empiezas a notar esos síntomas, recomiendo ir pensando en un formateo. La pregunta que se me viene en ese momento es ... ¿Pero hace cuanto que formatee? Pues aquí mi pequeño truco.

```
`ls -alct /|tail -1|awk '{print $6, $7, $8}'`
```

¿Te sirvio?
