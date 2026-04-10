---
title: "Varnish modifica las peticiones GET a peticiones POST"
date: 2012-12-02
tags: [varnish]
source: https://desarrolla2.com/post/varnish-modifica-las-peticiones-get-a-peticiones-post
---

# [Varnish modifica las peticiones GET a peticiones POST](https://desarrolla2.com/post/varnish-modifica-las-peticiones-get-a-peticiones-post)

Desde hacía mucho tiempo que vengo observando algunos problemas con el blog, por ejemplo, no estaba recibiendo comentarios.

Al ponerme a investigar descubro que las peticiones POST se estaban convirtiendo en peticiones GET por **arte y magia** de varnish.

Según tenía entendido Varnish no cacheaba las peticiones POST. Veamos el contenido de mi vlc.

```
`sub vcl_recv {

  set req.http.Host = regsub(req.http.Host, ":[0-9]+", "");
  set req.http.X-Forwarded-For = client.ip;
   if (req.http.X-Forwarded-Proto == "https" ) {
     set req.http.X-Forwarded-Port = "443";
   } else {
     set req.http.X-Forwarded-Port = "80";
     set req.http.X-Forwarded-Proto = "http";
   }
        if (req.http.Authorization) {
                return (pass);
        }

  if (req.url ~ "\.(jpg|jpeg|gif|png|ico|css|js)$") {
          unset req.http.Cookie;
  }

  return(lookup);
}
`
```

Un lookup es lo que debes regresar si quieres que Varnish trate de buscar en la cache.

En este caso Varnish después de comprobar que el recurso no existia, al ser la petición un POST solicitaba al backend una petción web que si puede almacenar, un GET. Aqui estaba el error.

La solución es comprobar que no estas enviando nada que no sea GET o HEAD a un antes de de devolver lookup.

```
`sub vcl_recv {

  // other stuffs

   if (req.request != "GET" && req.request != "HEAD") {
    return (pass);
  }

  return(lookup);

}
`
```

Dejo esto documentado por que creo que puede ser un error muy frecuente si estás empezando.
