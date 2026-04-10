---
title: "Fusionar commits con git"
date: 2012-03-28
tags: [git, bash]
source: https://desarrolla2.com/post/fusionar-commits-con-git
---

# [Fusionar commits con git](https://desarrolla2.com/post/fusionar-commits-con-git)

Con frecuencia me ocurre que hago un commit, y poco después me doy cuenta de que hay algo que quiero modificar y que está relacionado con el commit. La estrategia que utilizo para los mensajes en los commits, es poner "refs #123, #124" para indicar los tickets con los que está relacionado, usualmente solo es uno, y "fixed #123" para indicar que cierra un ticket. Después de esta notación incluyo si lo creo conveniente, algún detalle extra, "fixed #123, error en los permisos de la carpeta /web" Al final el historial de log, termina quedando muy sucio, con un montón de commits por ticket. Git de nuevo, nos ayuda con esta tarea.

```
git reset --soft HEAD~i
git commit -a -m "fixed #123, error en los permisos de la carpeta /web"

```

Donde i es el número de commits que quieres fusionar. Y listo, ya no tienes excusa para manter tu historial de logs bien limpio.
