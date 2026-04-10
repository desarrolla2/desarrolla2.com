---
title: "Extraer traducciones de tu aplicación symfony"
date: 2018-05-27
tags: [php, symfony, traduccion]
source: https://desarrolla2.com/post/extraer-traducciones-de-tu-aplicacion-symfony
---

Symfony cuenta con un componente para traducciones bastante potente. Sin embargo tiene una carencia a la hora de extraer las diferentes cadenas para que puedan ser traducidas.

De serie trae el comando translation:update que buscará en nuestros ficheros twig y añadirá las cadenas que no tengamos a un fichero de traducciones.

¿Pero qué ocurre por ejemplo con las traducciones de variables, constantes,  y otros valores que no se traducen desde una cadena de twig? Pues básicamente que no hay una buena solución para esto.

Nosotros tenemos el siguiente método para tratar con este espinoso problema.

1. Escribimos todos los mensajes en inglés.
2. Definimos una estructura de mensajes para evitar que se generen diferentes mensajes que significan lo mismo.
3. Escribimos los mensajes en minúsculas.

Siempre pasamos los mensajes a través del filtro trans y luego ponemos en mayúscula o capitalizado con filtros de twig.

```
`{{ 'hi %name%'|trans({’%nane%’: user.firstName})|capitalize }}.`
```

Hacemos lo mismo con los mensajes que se envían desde los controladores.

```
`{% for message in app.session.flashbag.get('error') %}
  <div class="alert alert-danger">{{ message|trans }}</div>
  {% endfor %}`
```

Y pasamos por los filtros correspondientes las fechas y los números

```
`{{ activity.date|localizeddate('medium', 'short') }}`
```

Como hemos dicho esto lo hacemos siempre, independientemente de si el sitio va a ser traducido a varios idiomas, así cuando nos piden que lo traduzcamos lo tenemos prácticamente listo.

El único problema que tenemos es encontrar todas esa cadenas que necesitan ser traducidas. Antes lo que hacíamos es cada vez que añadías una cadena, ir al fichero de traducciones, ver si estaba y si no estaba añadirla, pero esto es un trabajo muy tedioso.

A continuación la solución que proponemos.

Añadimos el siguiente servicio:

```
`services:
  monolog.formatter.translator:
    class: 'Monolog\Formatter\LineFormatter'
    arguments:
      - "%%context.id%%\n"`
```

Después actualizamos la configuración de monolog para los entornos de dev y test.

```
`monolog:
  handlers:
    translation:
      type: 'stream'
      path: "%kernel.logs_dir%/translation.missed.csv"
      level: 'debug'
      channels: ['translation']
      formatter: 'monolog.formatter.translator'`
```

Lo que hace este logger es dejar en un fichero csv todas las cadenas que el componente de traducción no pudo traducir.

Con el siguiente comando limpiamos el fichero de cadenas duplicadas.

```
`awk '!seen[$0]++' var/logs/translation.missed.csv | sort | tee var/logs/translation.missed.csv`
```

Ahora sólo hace falta subir este fichero a nuestro sistema de traducciones favorito.

¿Que te ha parecido?
