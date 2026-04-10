---
title: "Mi fichero de configuración de varnish"
date: 2013-10-29
tags: [linux, varnish, cache, http-cache, rendimiento]
source: https://desarrolla2.com/post/mi-fichero-de-configuracion-de-varnish
---

# [Mi fichero de configuración de varnish](https://desarrolla2.com/post/mi-fichero-de-configuracion-de-varnish)

Configurar Varnish, no es una tarea sencilla, en mi caso, fueron un monton de conceptos nuevos, y un montón de ejemplos, algunos de los cuales no funcionaban del todo, por lo que quiero publicar mi versión, para quien le pueda servir, y por si alguien quiere dejar alguna sugerencia.

A continuación el detalle de mi fichero de configuración de varnish, tened en cuenta, de que estoy utilizando la versión 3.0.x

### backends

Comienza definiendo los backend, en mi caso solo tengo un backend, que se trata de un nginx en la misma máquina, y en el puerto 8080.

```
backend default {
  .host = "127.0.0.1";
  .port = "8080";
}
```

### vcl_recv

vcl_recv es un "hook", algo así como un evento que sucede cuando la petición llega al vanirsh, y se a procesado correctamente, esto, es que tenemos un objeto req disponible que podemos usar, para tomar decisiones.

No voy a entrar en el detalle, pero en mi caso utilizo el hook para eliminar cookies, y decidir si la respuesta debe solicitarse al backend ( pass ) o buscar en la cache ( lookup ).

Gran parte del contenido, son pequeños snipped que he ido encontrando y afinando para mi propia configuración.

```
sub vcl_recv {

  # Serve objects up to 2 minutes
  set req.grace = 2m;    
  set req.http.X-Forwarded-For = client.ip;
  set req.http.Host = regsub(req.http.Host, ":[0-9]+", "");
  set req.backend = default;


  if (req.request == "PURGE") {
         error 405 "Not allowed.";

           return(lookup);
     }
        
  if (
    req.request != "GET" &&
        req.request != "HEAD" &&
        req.request != "PUT" &&
        req.request != "POST" &&
        req.request != "TRACE" &&
        req.request != "OPTIONS" &&
        req.request != "DELETE"
    ) {
    # Non-RFC2616 or CONNECT which is weird.

    return (pipe);
  }        
        
  if (req.request != "GET" && req.request != "HEAD") {
    # We only deal with GET and HEAD by default

    return (pass);
  }  

  # Remove cookies set by Google Analytics (pattern: '__utmABC')
    if (req.http.Cookie) {
    set req.http.Cookie = regsuball(req.http.Cookie, "(^|; ) *__utm.=[^;]+;? *", "\1");
    if (req.http.Cookie == "") {
      remove req.http.Cookie;
    }
  }

  if (req.http.X-Forwarded-Proto == "https" ) {
    set req.http.X-Forwarded-Port = "443";
  } else {
    set req.http.X-Forwarded-Port = "80";
    set req.http.X-Forwarded-Proto = "http";
  }

  # Search in cache
  if (req.url ~ "\.(jpg|jpeg|gif|png|ico|css|zip|tgz|gz|rar|bz2|pdf|txt|tar|wav|bmp|rtf|js|flv|swf|xml)$") {
    set req.http.user-agent = "Mozilla";
    unset req.http.Cookie;

    return(lookup);
  }

  # Normalize Accept-Encoding to reduce vary
  if (req.http.Accept-Encoding) {
    if (req.http.User-Agent ~ "MSIE 6") {
      unset req.http.Accept-Encoding;
    } elsif (req.http.Accept-Encoding ~ "gzip") {
      set req.http.Accept-Encoding = "gzip";
    } elsif (req.http.Accept-Encoding ~ "deflate") {
      set req.http.Accept-Encoding = "deflate";
    } else {
        unset req.http.Accept-Encoding;
    }
  }

    # Not cacheable by default
  if (req.http.Authorization /* || req.http.Cookie */) {     

    return (pass);
  }

  return(lookup);
}
```

### vcl_fetch

Este "hook" es el responsable de procesar una respuesta que ha sido enviada desde el backend, en este caso, elimino las cookies de los ficheros estaticos.

```
  if (req.url ~ "\.(jpg|jpeg|gif|png|ico|css|zip|tgz|gz|rar|bz2|pdf|txt|tar|wav|bmp|rtf|js|flv|swf|xml)$") {
    unset beresp.http.set-cookie;
  }

  return (deliver);

```

### Toques finales

Mi fichero de configuración de varnish contiene algunos detalles extra, sobre los que no quiero profundizar en esta entrada, como por ejemplo eliminar algunas cabezeras HTTP, y añadir algunas personalizadas.

En general tu fichero de configuración de varnish debería ser lo más agnostico de tu aplicación posible, es decir, que es necesario que utilizes las cabeceras HTTP, para indicarle a varnish como deseas que se comporte.

Espero que os sea util.

Puedes ver [aqui](https://gist.github.com/desarrolla2/7216729) la versión completa.
