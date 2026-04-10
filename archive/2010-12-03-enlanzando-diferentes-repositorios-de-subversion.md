---
title: "Enlanzando diferentes repositorios de subversion"
date: 2010-12-03
tags: [linux, subversion]
source: https://desarrolla2.com/post/enlanzando-diferentes-repositorios-de-subversion
---

# [Enlanzando diferentes repositorios de subversion](https://desarrolla2.com/post/enlanzando-diferentes-repositorios-de-subversion)

### ¿Que es svn:externals?

Es una propiedad de los repositorios subversion que permite enlazar diferentes repositorios, o incluso  partes de un repositorio en si mismo. Supongamos que tengo dos proyectos repo1 y repo2 y necesito que incluir el código del primero dentro del segundo. Gracias a svn:externals puedo decirle a mi subversion que quiero incluir repo1 dentro de repo2, sin necesidad de hacer un copiar y pegar y manteniendo la integridad de repo1.

### ¿En que casos es útil?

Es útil en multitud de casos, pero especialmente para incluir librerías externas a mi repositorio, por ejemplo symfony, la forma habitual de generar el proyecto es descargar todo el código fuente de symfony e incluirlo dentro de lib/vendor. ¿No es inútil tener una copia de symfony para cada proyecto si en todos ellos el código es el mismo?¿No es un tedioso actualizar symfony en todos los proyectos en los que trabajo? Hay una forma mejor de gestionar todo esto, svn:externals

### ¿Cómo funciona?

Pues es bastante sencillo, y una vez que aprendes como funciona, lo cierto es que es adictivo y quieres usar casi cualquier cosa como un external. Para seguir con el ejemplo vamos a ver como se crearía un proyecto symfony usando un external, generaremos un directorio y lo enlazaremos con la rama 1.4 de symfony

```
`mkdir -p lib/vendor/
svn propget svn:externals lib/vendor/`
```

se abrirá el editor de texto que tenga por defecto, añado la linea que enlaza con el repositorio externo symfony http://svn.symfony-project.com/branches/1.4 Si necesitara enlazar varios repositorios, cada uno debe estar en una linea diferente. svn up y ya puedes continuar normalmente.
