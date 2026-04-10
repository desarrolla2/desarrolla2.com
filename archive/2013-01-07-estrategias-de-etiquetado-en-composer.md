---
title: "Estrategias de etiquetado en composer"
date: 2013-01-07
tags: [php, git, composer, packagist]
source: https://desarrolla2.com/post/estrategias-de-etiquetado-en-composer
---

# [Estrategias de etiquetado en composer](https://desarrolla2.com/post/estrategias-de-etiquetado-en-composer)

Composer es un increible gestor de dependencias para php del que ya hemos hablado en este blog, y cada día más y más proyectos comienzan a usarlo.

Pero no todo está en composer, la principal responsabilidad recae en los desarrolladores de los paquetes, es decir, que los dolores de cabeza por problemas de dependencias, son normalmente probocados por una mala gestión de las mismas, por parte de los que mantienen los paquetes.

Principalmente he detectado tres tipos de estrategias en cuanto al uso de composer se refiere, y tambien añado cuales son mis impresiones sobre ellas.

## Viviendo en "dev-master"

El desarrollador del paquete no se preocupa por etiquetar  correctamente cada versión. Tristemente casi todos los bundles de symfony se encuentran en este caso.

Esta es una mala practica, incluso si tu software se encuentra en una fase de fuerte desarrollo, deberías usar etiquetas en el momento en el que el paquete es usado por otros, para poder garantizarles algo de estabilidad.

No es la primera vez que he realizado un composer update y mi código ha quedado roto, por cambios en librerías en las que sus desarrolladores no se han preocupado en pensar en los problemas que puede conllevar a los que están usando dichas librerías.

## Versionando en función del tiempo y/o de las nuevas características

Las versiones son etiquetadas cuando el desarrollador "siente" que debe hacerlo, es decir, que vive en dev-master y se preocupa de crear una etiqueta una vez que ha alcanzado una "meta", ya puede ser temporal, como por ejemplo cada seís meses o a completado una determinada caracteristica.

Este tipo de estrategias suelen ser útiles en paquetes que son utilizados masivamente.

Una critica a este tipo de estrategias, es que la corrección de un error, no es etiquetado, hasta que no se alcanza la siguiente "meta", y por lo tanto no será distribuida hasta dicho momento, salvo por los usuarios que decidan utilizar la rama "dev-master" de su software.

## Los frikis de las etiquetas

Un verdadero friki de las etiquetas añadirá una nueva etiqueta en cuanto pueda, es decir, en cuanto tenga una serie de commits, que generan una nueva versión estable de su paquete, etiquetandolo de la siguiente forma: *X.Y+1.0* si añadio una nueva caracteristica, o *X.Y.Z+1* si se corrigio algún error.

¿Que hay de problematico en sacar 5 nuevas versiones al día?  En la mayoría de los casos la respuesta sea nada, así que esta es la mejor estrategia que pueda permitir a los que utilizan tus paquetes mantener su código funcionando correctamente.
