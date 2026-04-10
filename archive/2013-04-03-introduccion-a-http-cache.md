---
title: "Introducción a HTTP Cache"
date: 2013-04-03
tags: [cache, http-cache, rendimiento]
source: https://desarrolla2.com/post/introduccion-a-http-cache
---

# [Introducción a HTTP Cache](https://desarrolla2.com/post/introduccion-a-http-cache)

HTTP Caché cumple un doble objetivo.

Mejora la latencia de tus sitios web, tus usuarios verán tus sitios web carga mucho más rápido.

Disminuye el tráfico de red y la carga de tus servidores, en otras palabras, tu servicio será más barato.

¡¡¡ Además es muy fácil de implementar !!!

# ¿Que és?

Es un conjunto de reglas de HTTP que te permiten indicar cuanto tiempo será válido un recurso, o si este no ha sido modificado desde la última solicitud.

Veremos más adelante y más profundamente estos dos conceptos.

## ¿Cómo funciona?

HTTP Cache implementa las reglas para que se genere un tipo de cache llamada “caché web”.

Estas cabeceras viajan con el recurso de la siguiente forma

```
HTTP/1.0 200 OK
Content-Type: text/html; charset=UTF-8
Cache-Control: public, s-maxage=1800
Last-Modified: Wed, 03 Apr 2013 13:22:21 GMT
Date: Wed, 03 Apr 2013 14:16:04 GMT
```

Puedes adivinar que estas cabeceras son las responsables de gestionar la cache.

La caché web puede estar alojada en diferentes sitios.

## Cache en el navegador del usuario

Esto es, el usuario solicita un recurso al servidor, lo almacena, y cuando lo vuelve a necesitar, el navegador lo recupera del disco duro del propio dispositivo. El tiempo de respuesta de este tipo de cache, es con diferencia el más rápido que existe.

## Cache en servidores proxy

Aunque no te des cuenta, es muy posible que existan varios servidores proxy entre tu y la web. A este tipo de proxy se les denomina “proxy transparente”.

Lo que nos interesa en este momento es la función que ejercen, cacheando determinados recursos, de forma que no se vuelven a solicitar.

## Tipos de cache web

Principalmente existen dos tipos de cache web, expiración y validación, vamos a ver como funciona cada una de ellas, y cómo funcionan cuando trabajan juntas.

### Frescura

Cuando un servidor web envía un recurso a un navegador o a otro sistema que lo está solicitando, añade una cabecera que indica que ese recurso será valido durante un determinado periodo de tiempo, por ejemplo 3600 segundos, o hasta una fecha determinada, por ejemplo 1 de Enero de 2014.

El navegador o el servidor proxy guardarán el recurso y no lo volverán a solicitar hasta que ese recurso no haya expirado, veamos a continuación cuales son las directivas

##### Expires

Esta directiva indica el momento hasta el cual el recurso será válido.

Tiene una forma similar a esto

Expires: Fri, 30 Oct 1998 14:19:41 GMT

##### Cache-Control.

Esta directiva es un poco más compleja, ya que puede tener una lista de propiedades separados por comas.

- 

max-age=[seconds] esta directiva es equivalente a Expires, e indica el número de segundos durante los cuales el recurso será válido.

- 

s-maxage=[seconds] es identica a la anterior, pero solo aplica a sistemas de cache compartida, por ejemplo los proxy cache.

- 

public indica que la respuesta de una petición es cacheable, por todo tipo de sistemas de cache (compartidos y no)

- 

private indica que la respuesta se generó para un único usuario, y por lo tanto no debe ser cacheada por sistemas compartidos pero si por ejemplo por un navegador web.

- 

no-cache indica que el sistema no debería cachear la respuesta

- 

no-store indica que no se debe guardar el recurso en la cache.

- 

must-revalidate: indica a los sistemas de cache si pueden utilizar el recurso una vez que este se encuentra caducado.

- 

proxy-revalidate — similar a must-revalidate, excepto que aplica sólo a sistemas proxy.

Cache-Control ofrece bastantes herramientas para gestionar la cache, tal y como tu necesites.

Veamos algunos ejemplos.

```
Cache-Control: public, max-age=3600
```

Esta es ejemplo típico, quiere decir, que todos los sistemas deben cachear la respuesta hasta dentro de una hora.

```
Cache-Control: max-age=0, must-revalidate
```

En el ejemplo indicamos que el recurso caduca en el momento de la respuesta, pero aún así un servidor proxy podría cachear, y regresar la misma respuesta si por ejemplo no se puede conectar con el servidor original o si este regresa un error 5xx.

Cuando una directiva Cache-Control y una directiva Expires está presente, Cache-Control es la que tiene validez.

### Validación

El otro tipo de directivas HTTP Cache son las directivas de validación. En lugar de entregar la respuesta e indicarle al sistema de cache, cuánto tiempo debe considerarse fresca y por lo tanto utilizar esa copia, cada vez que sea necesario, el sistema de validación ofrece una forma de que el servidor de cache, pueda comprobar si el recurso a cambiado.

#### Last-Modified

Esta directiva indica cuando se modificó por última vez un recurso, cuando el navegador realiza la petición envía la fecha de la última modificación del recurso que tiene. El servidor la compara con la fecha de última modificación actual, y si no ha cambiado, regresa un codigo 200, pero no regresa el recurso, indicando a la cache que utilize el que ya tiene.

#### ETag

Funciona exactamente igual que Last-Modified, solo que el etag es un identificador único del recurso, puede ser por ejemplo un md5 del mismo.

Cuando la directiva Last-Modified y Etag están presentes, Etag es la que tiene mayor prioridad.

Como puedes ver, las cabeceras de frescura son mucho más rápidas, ya que no se produce ningún tipo de petición, mientras que las cabeceras de validación te permiten un mayor control sobre lo que se almacena en la cache.

Referencias

- [http://www.mnot.net/cache_docs/](http://www.mnot.net/cache_docs/)
- [http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html)

Continuará

Durante los próximos días pienso seguir escribiendo sobre HTTP Cache, cuales son los problemas que tiene, cuales son las preguntas más frecuentes, como implementar fácilmente http cache, como utilizar inteligentemente frescura y validación, como implementar HTTP cache utilizando apache y php, y algunos consejos personales.

Pasate por aqui si quieres verlos.
