---
title: "Añadir binarios como vendors a través de composer"
date: 2013-01-31
tags: [composer]
source: https://desarrolla2.com/post/anadir-binarios-como-vendors-a-traves-de-composer
---

# [Añadir binarios como vendors a través de composer](https://desarrolla2.com/post/anadir-binarios-como-vendors-a-traves-de-composer)

En muchas ocasiones cuando te enfrentas a un problema es muy probable que este lo resuelva algún programa que ya existe y no desees lidiar con esta situación. En tiempos pretéritos lo que hacíamos era meter el binario "a pelo" dentro del sistema de control de versiones y a correr. 

Pero en el fondo, sabías que estabas haciendo mal. Hoy me he enfrentado a esa situación y he visto cómo composer permite solucionarlo de forma muy sencilla.

Para ello en el fichero composer.json, necesitas definir un bloque repositories en el que se añaden las fuentes externas. Aqui pueden ir diferentes tipos, desde otro [packagist](https://packagist.org/), un [satis](https://github.com/composer/satis) o un git, pero en el ejemplo que me ocupa se trata de un fichero comprimido.

```
{
    // ..
    "repositories": [{
            "type": "package",
            "package": {
                "name": "desarrolla2/myBinaryProgram",
                "version": "0.11.0_rc1",
                "dist": {
                    "url": "http://desarrolla2.com/myBinaryProgram.tar.gz",
                    "type": "tar"
                }
            }
        }
    ]
}
```

y una vez que hemos declarado el repositorio, añadirlo al bloque de require correspondiente.

```
{
    // ..
    "require": {
       "desarrolla2/myBinaryProgram" : "*"
    },
}
```

Y finalmente actualizar

```
composer update
```

Si quieres puedes leer la [referencia completa de repositorios](http://getcomposer.org/doc/05-repositories.md) en la documentación de composer.

Muy útil, para no tener que incluir binarios dentro del control de versiones, y que funcione tan ligero como nos gusta, nos vemos.
