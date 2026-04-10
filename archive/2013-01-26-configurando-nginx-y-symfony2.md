---
title: "Configurando Nginx y Symfony2"
date: 2013-01-26
tags: [symfony, nginx]
source: https://desarrolla2.com/post/configurando-nginx-y-symfony2
---

# [Configurando Nginx y Symfony2](https://desarrolla2.com/post/configurando-nginx-y-symfony2)

En este artículo vamos a ver como configurar un sitio web con Symfony2 y Nginx.

Si vienes como yo desde el mundo apache, trabajar con nginx puede parecerte un poco raro, pero en seguida te das cuenta que la sintaxis de nginx es mucho más clara y limpia.

Al igual que la configuración de apache suele estar en el fichero apache2.conf la de nginx se encuentra en nginx.conf, normalmente en la carpeta /etc/nginx.

En ese fichero está incluido esta directiva, es decir ejecutar todo lo incluido en la carpeta sites-enabled.

```
include /etc/nginx/sites-enabled/*;
```

Por convención debes generar tu sitio en la carpeta sites-available, con un aspecto parecido a esto.

```
server {
        listen   8080;
        root /var/www/desarrolla2.com/current/web;
        index app.php;

        server_name desarrolla2.com www.desarrolla2.com blog.desarrolla2.com;

        location / {
                try_files $uri $uri/ /app.php?$query_string;
        }
 
        location ~ \.php$ {
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index app.php;
                include fastcgi_params;
                fastcgi_param  SERVER_PORT        $http_x_forwarded_port;
        }

        location ~* ^.+\.(jpg|jpeg|gif|png|ico|swf|flv|woff)$ {
                access_log off;
                expires 9d;
        }

        location ~* ^.+\.(css|js)$ {
                access_log off;
                expires 1d;
        }
    
        access_log /var/log/nginx/access/desarrolla2.com.log;
        error_log /var/log/nginx/error/desarrolla2.com.log error;
}
```

Destacando lo más importante de arriba a abajo:

- La directiva server_name indica cuales son los host para los que se ejecutará este sitio, en este caso, el de mi blog, el sitio respondera a cualquiera de estos tres desarrolla2.com www.desarrolla2.com blog.desarrolla2.com
- La primera directiva location es una reescritura de url muy simple, no necesito más .htacces ni otras reescrituras como en apache, las cuales son bastante cripticas. En este punto ojo al final, de añadirle el parametro ?$query_string; ya que en la [documentación oficial](http://wiki.nginx.org/Symfony) de nginx está mal y sin esto se ignoran los parametros pasados por GET.
- La siguiente directiva location ~ \.php$ es la forma en la que le indico a nginx que todos los ficheros .php los tiene que ejecutar a través del socket de php-fpm
- Tambien es interesante lo facil que es añadir http cache a los ficheros de imagenes, css, y javascripts puede verse en las dos directivas siguientes. Tambien he desabilitado el log de acceso para estos recursos, ya que no tiene sentifo.
- Las últimas lineas son para indicar donde quiero que se generen los log, un detalle al respecto, y es que están en diferentes carpetas, por lo que luego es un poco más sencillo acceder a ellos.Espero que os haya sido útil.
