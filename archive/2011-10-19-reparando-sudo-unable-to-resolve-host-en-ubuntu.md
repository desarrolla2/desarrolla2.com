---
title: "Reparando 'sudo: unable to resolve host' en Ubuntu"
date: 2011-10-19
tags: [linux]
source: https://desarrolla2.com/post/reparando-sudo-unable-to-resolve-host-en-ubuntu
---

# [Reparando "sudo: unable to resolve host" en Ubuntu](https://desarrolla2.com/post/reparando-sudo-unable-to-resolve-host-en-ubuntu)

Este error se produce cuando en tu fichero /etc/hosts no hay una entrada para el nombre de la máquina sobre la que se está ejecutando. Me sucedio al sincronizar ficheros /etc/hosts entre diferentes máquinas, para que se propagaran las entradas y no tener que añadir cada cambio en todos a mano. La solución es sencilla, añadir la entrada correpondiente al nombre de la máquina, en mi caso:

```
::1vicent
127.0.1.1vicent
```
