---
title: "Eliminando .svn recursivamente"
date: 2010-12-02
tags: [linux, subversion]
source: https://desarrolla2.com/post/eliminando-svn-recursivamente
---

# [Eliminando .svn recursivamente](https://desarrolla2.com/post/eliminando-svn-recursivamente)

**¿Que es subversion?**[Subversion](http://subversion.tigris.org/) es uno de los sistemas de control de versiones, más utilizados. Los sistemas de control de versiones, o CVS por sus siglas en inglés son herramientas que permiten llevar un seguimiento sobre los cambios realizados en los ficheros, normalmente código.

En la actualidad, existen [CVS](http://es.wikipedia.org/wiki/CVS) más modernos, como git, o mercurial, pero subversion sigue siendo el elegido por multitud de empresas y desarrolladores, ya que con sus características se pueden realizar casi todas las operaciones.

### **¿Para que sirven las carpetas '.svn'?**

Las carpetas '.svn' son carpetas ocultas que no deberías modificar. Subversion las utiliza para llevar una gestión del estado de los ficheros con respecto al repositorio. Por ejemplo si has eliminado un conjunto de carpetas:

```
`svn del carpetas`
```

Subversion aún no las ha eliminado del sistema de ficheros, pero tiene un registro que le indica que en el próximo commit, debe eliminarlas

**¿Por que incordian tanto?** En realidad no incordian casi nunca, solamente cuando realizas determinadas operaciones manuales sobre tu copia de trabajo en las qu puedes dejar la copia de trabajo inservible, es decir, que no te deje hacer [commit](http://es.wikipedia.org/wiki/Commit_at%C3%B3mico), checkout u otras operaciones.

Una operación que puede dejar tu copia de trabajo en ese estado, es por ejemplo copiar ficheros o carpetas desde otra copia de trabajo, de forma que estarás mezclando o sobre escribiendo las carpetas '.svn', y por lo tanto dejando la información inconsistente. Tambien puede ser que simplemente deseas eliminarlas, por que no tienes intención de trabajar con ellas.

### **¿Entonces como las elimino?**

Fácil, solo ejecuta:

```
`rm -rf `find . -type d -name .svn``
```

Ahora ya no tienes una copia de trabajo, tan solo tienes un conjunto de ficheros, si quieres continuar trabajando con subversion, realiza un nuevo checkout y copia sobre el conjunto de carpetas y ficheros limpios.

```
`svn st`
```

puede ayudarte a saber como está todo.
