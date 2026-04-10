---
title: "Mi fichero de configuración de nginx (compatible con ImagineBundle)"
date: 2014-01-08
tags: [symfony, nginx]
source: https://desarrolla2.com/post/mi-fichero-de-configuracion-de-nginx-compatible-con-imaginebundle
---

# [Mi fichero de configuración de nginx (compatible con ImagineBundle)](https://desarrolla2.com/post/mi-fichero-de-configuracion-de-nginx-compatible-con-imaginebundle)

Hace unos días salio esté [artículo](http://egeloen.fr/2013/12/10/configure-nginx-with-avalanche-imagine-bundle/) en el que se detalla una configuración de nginx, compatible con ImagineBundle. Precisamente hace un par de semanas que también active ImagineBundle para este y [otros](https://desarrolla2.complanetubuntu.es) [sitios](https://desarrolla2.complanet-android.es) que tengo, asi que creo que puede ser interesante tambien compartir como lo resolví yo, además de detallar, el resto de la configuración.

### Eliminar www

Yo soy partidario de eliminar las www, ya que no aportan nada, y hacen el nombre de dominio, más largo, en este bloque que responde a todas las peticiones www.desarrolla2.com y blog.desarrolla2.com, con una redirección 301 a desarrolla2.com

```
server {
    listen   8080;
    server_name www.desarrolla2.com blog.desarrolla2.com;
    rewrite ^(.*) http://desarrolla2.com$1 permanent;
}

```

### Configuración

En este bloque, aparecen los datos generales de la configuración, el puerto que escicha, el nombre del servidor, el document root, y el fichero index.

```
    listen   8080;
    server_name desarrolla2.com;
    root /var/www/desarrolla2.com/current/web;
    index app.php;

```

### Redirigiendo las peticiones a Symfony2

El siguiente bloque redirige las peticiones al fichero app.php ( symfony2 ), además es importante no olvidar pasarle el parametro $query_string, ya que si no, se pierden los parametros pasados por url.

```
    location / {
        try_files $uri $uri/ /app.php?$query_string;
    }

```

### Habilitando ImagineBundle

Habilito que las peticiones de /media/cache, sean redirigidas tambien a app.php

```
    location ~ ^/media/cache {
        try_files $uri /app.php?$query_string;
    }

```

### Configuración de php en modo fastcgi

php-fastcgi es una configuración de ejecución de php, en modo servicio, es decir, que escucha peticiones en un determinado puerto, en lugar de estar como módulo de un servidor web.

```
    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index app.php;
        include fastcgi_params;
        fastcgi_param  SERVER_PORT        $http_x_forwarded_port;
    }
```

### Eliminanar cookies y añadir cabeceras http cache

Es responsabilidad de la aplicación y no del servidor web, realizar esta tarea, sin embargo en recursos (css, js, etc), será el servidor web, el encargado.

```
    location ~* ^.+\.(jpg|jpeg|gif|png|ico|swf|flv|woff)$ {
        access_log off;
        expires 30d;
    }
 
    location ~* ^.+\.(css|js)$ {
        access_log off;
        expires 7d;
    }
```

### Configurar Gzip

Habilitar gzip, reduce el tiempo de transferencia, a cambio de una pequeña ganancia en la carga del servidor. En este caso, además indico para que content-type quiero que se ejecute, por ejemplo no tiene sentido, comprimir un fichero zip, o un video.

```
    gzip on;
    gzip_vary on;
    gzip_types text/plain text/css application/x-javascript text/xml application/xml application/xml+rss text/javascript;

```

### Ficheros de log

Me gusta especificar carpetas diferentes para los logs, y los errores, ya que me permite por ejemplo realizar un tal *.log sobre la carpeta concreta, además de tener ficheros diferentes para cada sitio web.

```
    access_log /var/log/nginx/access/desarrolla2.com.log;
    error_log /var/log/nginx/error/desarrolla2.com.log error;

```

Si quieres ver la versión completa la tienes [aquí](https://gist.github.com/desarrolla2/8314651)
