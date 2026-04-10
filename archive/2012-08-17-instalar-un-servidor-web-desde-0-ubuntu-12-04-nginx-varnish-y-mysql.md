---
title: "Instalar un servidor web desde 0: ubuntu 12.04, nginx, varnish y mysql"
date: 2012-08-17
tags: [php, symfony, ubuntu, linux, bash, varnish, nginx]
source: https://desarrolla2.com/post/instalar-un-servidor-web-desde-0-ubuntu-12-04-nginx-varnish-y-mysql
---

# [Instalar un servidor web desde 0: ubuntu 12.04, nginx, varnish y mysql](https://desarrolla2.com/post/instalar-un-servidor-web-desde-0-ubuntu-12-04-nginx-varnish-y-mysql)

Acabo de reinstalar mi servidor web, y a continuación voy a detallar cuales son los pasos necesarios que he ido ejecutando para dejar el servidor tal y como quería.

## Opciones de configuración básicas

Lo primero de todo es actualizar los repositorios y los paquetes por si el sistema no está a la última versión.

```
`apt-get update
apt-get upgrade`
```

Cambiar la contraseña que viene por defecto en el equipo.

```
`passwd`
```

El siguiente paso es instalar algunas utilidades sin las cuales no me encuentro cómodo.

```
`apt-get install vim`
```

El servidor se encontraba en el timezone UTC, y prefiero tenerlo en el mismo que en el horario local en el que voy a trabajar, esto evitará algunos problemas de fechas, puedes ver cual es el timezone de tu equipo ejecutando.

```
`cat /etc/timezone`
```

Y puedes configurar el timezone local.

```
`echo 'Europe/Madrid' > /etc/timezone`
```

Instalé el demonio ntpd para mantener la hora del servidor actualizada.

```
`apt-get install ntpd
apt-get remove ntpdate`
```

Creamos un alias para el buzon de correo del usuario root, para que el sistema pueda enviarnos mensajes.

```
`#/etc/aliases
# Other aliases
root:           daniel.gonzalez@mi-dominio.es`
```

## Mysql

Instalamos los paquetes.

```
`apt-get install mysql-server mysql-client`
```

La instalación por defecto de mysql puede venir sin contraseña de root, como es mi caso.

```
`USE mysql;
UPDATE user SET password=PASSWORD('nuevo_pass') WHERE user='root';`
```

Me gusta poder conectarme de forma remota a MySQL, para ejecutar scritps, backups, ect, por lo que tengo que realizar algunos ajustes.

```
`USE mysql;
UPDATE user  set host = '%' where user = 'root' and host = 'ubuntu-12'; `
```

Edito el fichero de configuración de mysql y comento la siguiente linea.

```
`#/etc/mysql/my.conf
#bind-address           = 127.0.0.1`
```

Reinicio el servicio.

```
`/etc/init.d/mysql restart`
```

## Nginx y php-fpm

Nginx no interactua exactamente igual que apache con php, php-fpm es un servio igual que nginx, al que este enviará las peticiones que le indiquemos para que sean evaluadas por el interprete.

Instalamos nginx, y php y desinstalamos apache.

```
`apt-get remove apache2
apt-get install nginx php5-fpm php5-cli php5-suhosin php5-myqsql`
```

Seguramente quieras instalar más módulos de apache, puedes ver cuales hay disponibles.

```
`apt-cache search php | grep php`
```

Además si piensas utilizar composer, tendrás que editar lo siguiente en el fichero de configuración de suhosin.

```
`;/etc/php5/conf.d/suhosin.ini
suhosin.executor.disable_eval = On`
```

Vamos ahora a conectar ambos servicios a traves de web sockets. Editamos el fichero /etc/nginx/sites-enabled/default para que nos quede de esta manera.

```
`server {

        root /var/www/tu_sitio_por_defecto/current/web;
        index app.php index.php index.html;

        location ~ \.php$ {
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index index.php;
                include fastcgi_params;
        }
}`
```

Indicandole a Varnish que es lo que tiene que derivar a php-fmp y donde se encuentra. Nos aseguramos que php-fpm tiene la misma configuración de socket en /etc/php5/fpm/pool.d/www.conf.

```
`;listen = 127.0.0.1:9000
listen = /var/run/php5-fpm.sock`
```

Y reiniciamos los dos servicios.

```
`/etc/init.d/nginx restart
/etc/init.d/php5-fpm restart`
```

Continuará en los próximos días ...
