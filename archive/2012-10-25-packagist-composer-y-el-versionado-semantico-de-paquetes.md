---
title: "Packagist, Composer y el versionado semantico de paquetes."
date: 2012-10-25
tags: [php, git, composer, packagist]
source: https://desarrolla2.com/post/packagist-composer-y-el-versionado-semantico-de-paquetes
---

# [Packagist, Composer y el versionado semantico de paquetes.](https://desarrolla2.com/post/packagist-composer-y-el-versionado-semantico-de-paquetes)

El versionado semantico consiste en unas reglas sencillas en las que vas etiquetando tu desarrollo a medida que va evovolucionando.

Packagist, puede obtener información sobre tus releases desde el fichero composer.json, pero hay a mi manera una forma mucho más eficaz de solucionar el problema, a través de los tag en tu repositorio.

Primero veamos cuales son estas reglas. Los paquetes tienen dos partes, la primera se compone de tres números en formato "X.Y.Z" o "vX.Y.Z". La X determina la versión "mayor" es decir, cuando se hacen grandes cambios en la api de tu software. La Y determina la versión menor de tu software, y permitirá identificar pequeños cambios en la api, por ejemplo a partir de la 1.2.X tenemos disponible, el metodo ::*newMethod*(), y normalmente no debería romper la compatibilidad hacia atrás.

Finalmente la Z indicará el numero de parche, y sirve para determinar, que se ha corregido uno o más bugs, pero que no existen cambios en el API.

Además de esto, podemos añadir algunas etiquetas especiales, como "alpha", "beta", "rc".

¿Por que versión empiezo? Según este criterio deberías empezar por una versión "v0.1.0-alpha" y continuar tu desarrollo hasta lograr una versión "v1.0.X".

Lo bueno de todo esto es que composer, packagist y github sincronizan tus versiones a través de los "tag" de git, para crear un nuevo tag

```
`git tag -a "v0.4.17-rc1" -m "tag message description"`
```

Ahora solo tienes que empujar tus tag a github

```
`git puss --tag`
```
