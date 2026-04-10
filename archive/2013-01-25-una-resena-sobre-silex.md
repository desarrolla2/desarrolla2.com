---
title: "Una reseña sobre Silex"
date: 2013-01-25
tags: [php, symfony, silex]
source: https://desarrolla2.com/post/una-resena-sobre-silex
---

# [Una reseña sobre Silex](https://desarrolla2.com/post/una-resena-sobre-silex)

Los micro frameworks están de moda en PHP, entre otras opciones que están apareciendo nos encontramos [Silex](http://silex.sensiolabs.org/) sobre el que vamos a comentar en este artículo,  también hay otros como [Slim](http://www.slimframework.com/), [Flight](http://flightphp.com/), [Wave](http://www.waveframework.com/), [GluePHP](http://gluephp.com/) y estoy seguro de que a esta lista puedes añadirle unos cuantos más.

Tenía ganas de probar un proyecto con Silex, ver que ofrece y cuales son sus carencias, me interesaba sobre todo ver cuales son los motivos por lo que está tan de moda este tipo de frameworks.

La semana pasada realicé un proyecto personal simple que me permitiera realizar algunas estadísticas y análisis de datos, es decir, almacenar registros y pintarlos bonitos.

Tras el desarrollo de este pequeño proyecto he llegado a las siguientes conclusiones.

## Silex es un MicroFramework

Con esto quiero decir, que Silex trae lo básico, no trae ORM, no trae formularios, ni validación, ni seguridad, ni motor de plantillas, en general, no trae casi nada de lo que vas a poder necesitar en un proyecto web. Un MicroFramework, en general lo unico que hace es "bootstraping" del entorno y manejar la petición y la respuesta.

A la vez Silex es un framework con el que muy fácilmente puedes utilizar componentes de Symfony2, y está muy bien documentado, por lo que es muy probable que por muy pequeño que sea el proyecto acabes incluyendo tres o cuatro componentes de Symfony2.

En mi proyecto traté de incluir nada más que lo imprescindible y acabé incluyendo, solo twig, y el componente de seguridad. Silex ya incluye de por sí el http-kernel, el http-foundation y el event-dispacher. En el [ejemplo](https://github.com/javiereguiluz/bilbostack/blob/master/composer.json) de Javier Eguiluz, por ejemplo acaba incluyendo 14 componentes de Symfony2.

## Silex distribución estandar

En symfony comenzamos nuestros proyectos utilizando una distribución, normalmente la [symfony-standar](https://github.com/symfony/symfony-standard) con esto obtenemos la estructura de carpetas más habitual. En Silex no  existe este concepto, lo que significa que estás un poco más solo en la forma en la que puedes construir tu proyecto.

En el sitio web de Silex aparece mucha documentación, sin embargo he visto diferentes proyectos realizados con Silex y está claro que aunque se parecen no sigen exactamente la misma estrucura. Pueden verse por ejemplo las diferencias en cuanto a estructura entre estos proyectos.

- [Bilbostack](https://github.com/javiereguiluz/bilbostack) de Javier Eguiluz
- Este [acortador de URLs](https://github.com/khepin/tsusbos)
- Este [ejemplo](https://github.com/lyrixx/Silex-Kitchen-Edition/blob/master/web/index.php)

En general, y no se si esto es un error, en mis proyectos trato de seguir la misma estructura que sigen los proyectos en Symfony2, por ejemplo la configuración esta en app/config, las vistas están en app/Resources/views/ etc.

[**Corregido**] Si existe una distribución estándar de Silex, está en [Silex-Skeleton](https://github.com/fabpot/Silex-Skeleton), quizá me despistaron post como [este](http://php-and-symfony.matthiasnoback.nl/2012/01/silex-getting-your-project-structure-right/), también hay otras distribuciones no extandar.

## MicroORM

He escuchado que algunas personas reclamaban un microORM para Silex, no acabo de estar de acuerdo, lo mejor es que utilices PDO, y así no pierdes la oportunidad de cambiar sistema de gestión de base de datos. La [otra opción](http://silex.sensiolabs.org/doc/providers/doctrine.html) es utilizar DBAL de doctine.

## ¿Cuando usar Silex?

Realmente Symfony2 es un monstruo, comenzar un proyecto en Symfony2 requiere configurar un montón de cosas y descargar un montón de librerías. Ejecutar un composer install por primera vez, significa hacer una parada para tomar café. Con Silex puedes tener algo funcionando en menos de una hora.

Al menos en mi opinión Silex es una opción muy válida para proyectos muy pequeños y sobre todo para prototipos, muy posiblemente si tu proyecto crece vas a querer pasar el proyecto en Symfony2.

No lo he probado todavía, pero creo que puede ser ideal para el desarrollo de APIS, por ejemplo API REST.

Silex es muy extensible, si tan solo necesitas algunas de las caracterísiticas de Symfony2, puedes añadirlas con solo algunas líneas de configuración, y algunos require en el composer.json.

## ¿Cuando no usar Silex?

Si crees que tu proyecto va a requerir un desarrollo completo, autenticación, formularios, varias entidades, etc, ... casi seguro que te interesa utilizar Symfony2.

¿A tí qué te parece?
