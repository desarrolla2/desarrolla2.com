---
title: "Comparando ficheros con meld"
date: 2011-12-16
tags: [git, linux]
source: https://desarrolla2.com/post/comparando-ficheros-con-meld
---

# [Comparando ficheros con meld](https://desarrolla2.com/post/comparando-ficheros-con-meld)

[Meld](http://meld.sourceforge.net/) es una herramienta visual para comparar diferencias. Entre sus caracterisitcas se encuentra la posibilidad de comparar más de dos ficheros simultaneamente, editarlos y guardarlos desde el propio meld. Como veremos más adelante podemos utilizar meld junto con git y otros sistemas de control de versiones como bazaar o mercurial. Meld es una herramienta imprescindible para comparar diferencias.

## Instalar meld

Instalar medl es sencillo. Si utilizas ubuntu:

```
apt-get install meld
```

Si utilizas Fedora:

```
sudo yum install meld
```

Si utilizas cualquier otro sistema operativo, puedes mirar la pagina de [instrucciones de instalación](http://meld.sourceforge.net/install.html) de meld

## Primeros pasos con meld

### Diferencias entre dos ficheros

Para comparar dos ficheros que tienes en tu sistema de ficheros ejecuta el siguiente comando

```
meld file1.txt file2.txt
```

uedes encontrar fácilmente las diferencias entre ambos ficheros, en verde se muestran las lineas añadidas y en rojo las lineas eliminadas. Con las flechas que aparecen, mueves una linea de un fichero a otro. Por supuesto puedes editar cualquiera de los dos ficheros mediante meld, si por ejemplo ninguna de las dos opciones te convence.

### utilizar la tecla control

La funcionalidad de la tecla control es muy util, y no la conocía en otros editores de diferencias, por ejemplo en netbeans, o te quedas un fichero u otro, pero con meld puedes quedarte con las dos versiones a la vez de un cambio. Para ello pulsa control y meld añadira el cambio en el fichero B sin borrar el cambio del fichero A.

## Diferencias entre tres ficheros

Meld permite comparar diferencias entre cualquier número de ficheros, promocionando los cambios de uno a otro.

```
meld file1.txt file2.txt file3.txt ...
```

## Diferencias entre directorios

Otra caracteristica de meld que ya hemos comentado es la posibilidad de comparar directorios.

```
meld directory1/ directory2/
```

En este caso, te permitirá ver cuales son los ficheros añadidos, eliminados y cuales han sido modificados y tienen diferencias.

## Git

La caracteristica que más me gusta de meld es la posibilidad de utilizarlo junto con git, para obtener un control total de que a ocurrido en cada fichero. Por supuesto, gitg, netbeans u otras herramientas de git pueden darte algo de información, pero con meld, puedes comparar revisiones y quedarte con los cambios que consideres oportunos de una forma sencilla. Con el siguiente comando le indicas a git, que quieres que utilize meld para comparar las diferencias:

```
git config --global diff.external meld
```

En este momento git diff generará un error, pues git envia los parametros a meld en un orden diferente al esperado, por ello necesitas crear este fichero en tu directorio home/scripts:

```
#!/usr/bin/python
import sys
import os
os.system('meld "%s" "%s"' % (sys.argv[2], sys.argv[5]))

```

y actualizar diff external

```
git config --global diff.external /home/dgonzalez/Dropbox/scripts/git_diff.py
```

No olvides asignarle permisos de ejecución.

### Diferencias entre el ultimo commit y los cambios actuales

Ahora viene la magia ... tan solo entrar en el directorio de trabajo y

```
git diff
```

Para ver todos los cambios o

```
git diff file
```

Para ver un fichero o directorio en concreto.

### Diferencias entre dos ramas

```
git branch_A branch_b
```

### Diferencias entre dos commits

```
git hash_commit_1 hash_commit_2
```

### Limitando la comparación de diferencias

Si tan solo quiero ver, parte de las diferencias podria por ejemplo

```
git branch_A branch_b ./lib/myLibs
```
