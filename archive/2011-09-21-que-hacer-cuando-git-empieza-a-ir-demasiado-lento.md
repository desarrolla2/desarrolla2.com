---
title: "¿Que hacer cuando git empieza a ir demasiado lento?"
date: 2011-09-21
tags: [git]
source: https://desarrolla2.com/post/que-hacer-cuando-git-empieza-a-ir-demasiado-lento
---

# [¿Que hacer cuando git empieza a ir demasiado lento?](https://desarrolla2.com/post/que-hacer-cuando-git-empieza-a-ir-demasiado-lento)

Una de las caracteristicas de las que presume git es de ser rápido. Pero por experiencia, en repositorios grandes, con mucho historial, y especialmente con el paso del tiempo git empieza a funcionar un poco más lento cada vez. Incluso un git status se hace muy lento. Estaba teniendo problemas hasta que descubrí que con:

```
git gc --aggressive
```

Git se toma un tiempo para actualizar y optimizar sus indices, siendo el resultado una mejora considerable en los tiempos de respuesta.
