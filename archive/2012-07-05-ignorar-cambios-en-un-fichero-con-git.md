---
title: "Ignorar cambios en un fichero con git"
date: 2012-07-05
tags: [git]
source: https://desarrolla2.com/post/ignorar-cambios-en-un-fichero-con-git
---

# [Ignorar cambios en un fichero con git](https://desarrolla2.com/post/ignorar-cambios-en-un-fichero-con-git)

En ocasiones es posible que quieras modificar un fichero pero no deseas incluir sus cambios dentro del repositorio. Normalmente en ficheros de configuración.

```
`git update-index --assume-unchanged <file>`
```

Una vez que quieras que git tenga en cuenta de nuevo el fichero, ejectuta

```
`git update-index --no-assume-unchanged <file>`
```
