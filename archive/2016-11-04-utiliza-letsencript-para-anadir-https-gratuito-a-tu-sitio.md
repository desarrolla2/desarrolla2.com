---
title: "Utiliza LetsEncript para añadir HTTPS gratuito a tu sitio"
date: 2016-11-04
tags: [nginx, https]
source: https://desarrolla2.com/post/utiliza-letsencript-para-anadir-https-gratuito-a-tu-sitio
---

Hace algún tiempo quería comenzar a usar [letsencrypt](https://letsencrypt.org/). Pero me daba un poco de pereza ponerme manos a la obra y siempre lo estaba postergando un poco. Finalmente esta mañana me puse con ello, a continuación un resumen de como se hace.

Lo primero es descargar el binario, en mi caso lo dejo en /usr/local/sbin para poder ejecutarlo más adelante, tanto para actualizar el certificado como para añadir nuevos certificados.

```
`cd /usr/local/sbin
sudo wget https://dl.eff.org/certbot-auto
sudo chmod a+x /usr/local/sbin/certbot-auto`
```

Es necesario publicar el directorio .well-know dentro de tu nginx, edita el fichero pertinente a la configuración de tu dominio, en mi caso /etc/nginx/sites-enabled/desarrolla2.com.conf. letsencrypt necesita este paso para comprobar que tu eres el propietario del dominio, algo parecido al paso que tienes que hacer para añadir tu sitio a google webmaster tools.

En mi caso tengo varias reglas .location simplemente pon esta a continuación de las demás.

```
`server {
    ...
    location ~ /.well-known {
        allow all;
    }
    ...
}`
```

Antes de reiniciar nginx, comprueba que no has roto nada.

```
`sudo nginx -t`
```

Reinicia o recarga el servicio para que los cambios sean aplicados.

```
`sudo service nginx restart`
```

Ahora ejecuta lo siguiente. No olvides de poner tu webroot-path apuntando al directorio raiz de tu sitio, y añadir tantos parámetros -d como dominios quieras que estén en el certificado. 

```
`certbot-auto certonly -a webroot --webroot-path=/usr/share/nginx/desarrolla2.com/web -d desarrolla2.com -d www.desarrolla2.com`
```

Ya casi lo tienes aparecerá un mensaje donde te dirá que indiques una dirección de correo electrónico y aceptes las condiciones de uso.

// -- imagen here

Te va a generar una serie de ficheros, puedes verlos ejecutando:

```
`ls -l /etc/letsencrypt/live/desarrolla2.com`
```

Edita de nuevo tu fichero de configuración del sitio en nginx y añade lo siguiente dentro del bloque server.

```
`server {
    ...
    # Al principio del todo, quitando las otras entradas listen
    listen 443 ssl;
    ...
    # Al final del todo añade
    ssl_certificate /etc/letsencrypt/live/desarrolla2.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/desarrolla2.com/privkey.pem;
    ...
}`
```

Casi estamos terminando. En mi caso quiero forzar la conexión a través de https, ya que tengo certificado vamos a forzar su uso, para ello necesito hacer un 301 desde las peticiones http hacia las https, por lo que añadimos lo siguiente al final del fichero.

```
`server {
    listen 80;
    server_name desarrolla2.com www.desarrolla2.com;
    return 301 https://$host$request_uri;
}`
```

Comprueba que todo está bien y luego reinicia nginx.

```
`sudo nginx -t
sudo service nginx restart`
```

Asegúrate de probar que todo está funcionando bien y que no te has dejado nada accediendo a través de tu navegador.

Los certificados de letsencryt caducan al poco tiempo, por lo que necesitamos que periodicamente los certificados sean renovados, vamos a usar para ello crontab. Añade las siguientes lineas.

```
`30 2 * * 1 /usr/local/sbin/certbot-auto renew >> /var/log/le-renew.log
35 2 * * 1 /etc/init.d/nginx reload`
```

Ya lo tienes!
