---
title: "Conectarse a mysql a través de un túnel ssh"
date: 2017-08-21
tags: [linux, mysql, ssh, seguridad]
source: https://desarrolla2.com/post/conectarse-a-mysql-a-traves-de-un-tunel-ssh
---

El objetivo es encontrar una forma en la que poder acceder a las diferentes bases de datos con las que trabajamos, encontrando un punto de equilibrio entre seguridad y comodidad a la hora de trabajar.

Un [túnel ssh](https://es.wikipedia.org/wiki/T%C3%BAnel_(inform%C3%A1tica)) es una conexión que abres entre dos máquinas de forma que la comunicación entre ellas viaja encriptada.

Yo utilizo esta técnica para securizar las conexiones con los servidores mysql. Estos los tengo configurados de forma que sólo acepten conexiones locales. Por supuesto los tengo configurados con las directrices de [instalación segura.](https://dev.mysql.com/doc/refman/5.7/en/mysql-secure-installation.html)

Bueno lo que hago para conectarme a mis servidor mysql es abrir un túnel ssh de forma que abro una conexión remota en el puerto 3306 contra un puerto local, por ejemplo el 3307.

```
`ssh -N -L 3307:127.0.0.1:3306 root@mysql1.devtia.com &`
```

Lo que hace este comando es crear una comunicación entre nuestro puerto 3307 local y el puerto 3306 en el host remoto, por lo que si yo me conecto al 3307 local sería equivalente a si lo estuviera haciendo al 3307 remoto.

El último carácter el “&” lo que hace es [enviar ese proceso al background](https://serverfault.com/questions/41959/how-to-send-jobs-to-background-without-stopping-them) de forma que no necesito tener ocupada esa sesión de terminal.

Ahora para conectarme a ese servidor, puedo hacerlo vía línea de comandos.

```
`mysql -u root -p --port=3307`
```

Yo en mi caso normalmente utilizo phpMyAdmin, que me parece un cliente buenísimo. En otro post contaré como lo tengo configurado. Puedes usar cualquier cliente con el que te sientas cómodo.

Otra opción que a veces utilizo y es muy útil, es configurar mi proyecto local con el que estoy desarrollando contra la base de datos de producción.
