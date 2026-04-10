---
title: "Estado del repositorio git desde el promt de la linea de comandos."
date: 2011-09-20
tags: [git]
source: https://desarrolla2.com/post/estado-del-repositorio-git-desde-el-promt-de-la-linea-de-comandos
---

# [Estado del repositorio git desde el promt de la linea de comandos.](https://desarrolla2.com/post/estado-del-repositorio-git-desde-el-promt-de-la-linea-de-comandos)

Con el siguiente hack podemos ver el estado del repositorio git desde el promt de la linea de comandos. Si normalmente obtenemos algo parecido a esto:

```
dgonzalez@vicent ~$
```

Ahora tendremos algo parecido a esto:

```
dgonzalez@vicent ~/NetBeansProjects/d2symblog[master]$
```

Donde [master] es la rama en la que estamos trabajando, y en caso de necesitar realizar un commit nos mostrará [master*]. Para instalar este plugin, tan solo necesitas, copiar y pegar este fragmento de código al final de tu .bashrc situado en tu directorio home.

```
function parse_git_dirty {
  [[ $(git status 2> /dev/null | tail -n1) != "nothing to commit (working directory clean)" ]] && echo "*"
}
function parse_git_branch {
  git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e "s/* \(.*\)/[\1$(parse_git_dirty)]/"
}
export PS1='\u@\h \[\033[1;33m\]\w\[\033[0m\]$(parse_git_branch)$ '
```

Sólo te queda salir de la sesión ( de la terminal ) y volver a entrar para que los cambios tengan efecto.
