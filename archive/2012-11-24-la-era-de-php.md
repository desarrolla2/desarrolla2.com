---
title: "La era de PHP"
date: 2012-11-24
tags: [php, symfony]
source: https://desarrolla2.com/post/la-era-de-php
---

# [La era de PHP](https://desarrolla2.com/post/la-era-de-php)

Cuando comencé a programar PHP era un lenguaje con el que era muy sencillo hacer un sitio web, con tan solo un editor de texto y un cliente de ftp. Si además eras capaz de manejar algún CMS entonces eras la ostia. Por lo tanto era la forma más fácil y rápida de comenzar a hacer sitios web.

El ecosistema estaba muy fragmentado, y carecía de las herramientas y sobre todo de la estandarización y buenas practicas que en otros lenguajes funcionaban desde hacia mucho.

Durante los últimos años el ecosistema de PHP ha avanzado rápidamente,  A día de hoy PHP es una opción tan válida como cualquier otra para realizar desarrollos web profesiones y de calidad.

## Soporte para la Orientación a Objetos

PHP nació en el año 1995 y fue uno de los primeros lenguajes que podían ser incorporados directamente sobre el código HTML. en el año 2000 sale a la luz PHP4, con un soporte para la orientación a objetos muy pobre.

Con la llegada de PHP5, en el 2004 comenzó un lento cambio en cuanto a la forma de utilizar PHP se refiere. Se añadió mayor soporte para orientación a objetos y se añadieron un montón de nuevas extensiones a la SPL como el manejo de XML, Soap, SQlite, Iteradores, Excepciones, se mejoró mucho el rendimiento del lenguaje y de sus extensiones.

Comienza a plantearse en este momento a PHP como un competidor de otros lenguajes en el entorno empresarial, prueba de ello es que las editoriales comenzaron a publicar libros sobre PHP orientado a objetos y buenas prácticas como por ejemplo [PHP Objects, Patterns, and Practice](http://www.amazon.es/Objects-Patterns-Practice-Matt-Zandstra/dp/1590599098/ref=sr_1_8?ie=UTF8&qid=1353882426&sr=8-8), o  [Pro PHP: Patterns, Frameworks, Testing and More](http://www.amazon.es/Pro-PHP-Patterns-Frameworks-Testing/dp/1590598199/ref=sr_1_15?ie=UTF8&qid=1353882426&sr=8-15).

Pero aún faltaba algo y esto llegó con PHP5.3 con mejoras como los namespaces, funciones lamda , clousures, se reescribio el recolector de basura, y se realizaron muchas otras mejoras.

En Marzo de 2012 vio la luz PHP5.4 con Traits, mejoras en el type hynting, array deferencing, y otros, aunque lo cierto es que PHP5.4 no esta totalmente extendido, y muchos sistemas de hosting, o no lo soportan, o lo soportan solo hace unos meses.

En el tintero nos queda PHP5.5 del que ya tenemos una alpha.

## Seguridad

En la comunidad temprana de PHP predominaban webmaster que sin conocimientos en desarrollo web, utilizaban sistemas CMS, Foros y otras herramientas Open Source, que contenían grabes deficiencias de seguridad. Además instalaban multitud de extensiones de terceros.

Con el paso del tiempo la comunidad ha madurado y ha ido tomando conciencia, por lo que los proyectos Open Source (Drupal, WordPress, etc ... ) tienen la seguridad como una de sus principales preocupaciones.

La comunidad se ha preocupado de concienciarse a sí misma y hoy cualquiera sabe lo que es un XSS, un SQLInjection, sabe que las paswords no se almacenan en texto plano, y  en general que hay que validar toda la información que proviene de fuera de nuestro sistema.

La comunidad de PHP se ha puesto las pilas y prueba de ello es el  [PHP Consortium](http://phpsec.org/) o el bloque dedicado a seguridad del proyecto [PHP the right way](http://www.phptherightway.com/#security)

## Estandares 

Como ya hemos comentado PHP fue en sus comienzos un lenguaje muy fragmentado, la curva de aprendizaje entre los diferentes proyectos era mas alta de lo normal, ya que cada uno hacía las cosas a su manera.

Para resolver esto se creo un grupo con las personas que lideraban los principales proyectos en PHP y crearon el "[PHP Framework Interoperability Group](https://github.com/php-fig/fig-standards)" para proponer y definir unos estándares de codificación que hicieran más fácil el trabajar con diferentes frameworks, ya que todos ellos mantendrían algunas determinadas convenciones en común.

Este estándar se lee en un ratito y es de obligada lectura.

## Testing

Con el aumento de la complejidad del tipo de proyectos que se desarrollaban en PHP, era necesario un ecosistema de testing maduro. [PHPUnit](https://github.com/sebastianbergmann/phpunit/) se ha convertido en el estándar para la realización de pruebas. Pero el ecosistema va mucho más alla, con sistemas complementarios como [Mockery](https://github.com/padraic/mockery) para pruebas unitarias, y otros como [Behat](http://behat.org/) o [Codeception](http://codeception.com/) para realizar BDD.[  
](http://behat.org/)

Tambien se ha trabajado en frameworks para pruebas funcionales, aunque en este sentido cada framework suele implementar los suyos, a mi personalmente me gusta bastante el sistema que tiene [Symfony2](http://symfony.com/doc/current/book/testing.html#working-with-the-test-client).

Hay un montón de libros sobre testing en PHP, pero si tuviera que recomendar uno, sería el libro "[diseño ágil con TDD](http://www.dirigidoportests.com/el-libro)" aunque este no esté para PHP.

## Frameworks

En [wikipedia](http://en.wikipedia.org/wiki/Category:PHP_frameworks) aparecen documentados 18 frameworks PHP, que con las mejoras en la programación orientada a objetos, el uso de estandares y la difusión de sistemas de pruebas, se han convertido en sistemas maduros, con una gran conciencia sobre el uso de buenas prácticas y patrones de diseño.

Si tuviera que destacar algunos, [CakePHP](http://cakephp.org/), [ZendFramework](http://framework.zend.com/), y [Symfony](http://symfony.com/) creo que son los que se encuentran posiblemente entre los mejores, pero todos han contribuido a llegar hasta este momento.

Pero esto no significa que estos frameworks pueden dormirse en los laureles, por que aparecen nuevos frameworks como [Aura](http://auraphp.github.com/) que cogen lo mejor de cada uno de ellos, y le añaden nuevas características como soporte para PHP5.4 o mayor desacoplamiento entre sus componentes.

Casi podemos decir, que se está iniciando una "guerra de los frameworks" en PHP, de la que puede salir beneficiada la comunidad.

## Composer

Uno de los problemas a los que se enfrentaba un proyecto en php al intentar realizar un desarrollo modular, es que no existía un buen pegamento para montar el chiringuito. Composer ha solucionado este problema, puedes coger cualquier librería que siga el SPR-0 y composer la añade a tu proyecto y genera un autoloading, especifico para el.

Ahora no solo utilizas el framework que te gusta si no cualquier componente o libreria que te encuentres por hay y que te resulte interesante de una forma muy sencilla.

Esto a mi parecer genera una comunidad mucho mas promiscua, es decir, que si bien usas FrameworkA, para ti es muy sencillo acudir a FrameworkB, cojer lo que necesitas y usarlo, con lo que tenemos mayor enriquecimiento de la comunidad, y re utilización de los componentes buenos.

A la vez los Frameworks y librerías se retroalimentan de esto a través de feedback y envío de parches y errores.

## Y en el futuro ...

Nos esperan muchas cosas nuevas, pero sobre todo, muchas cosas buenas que no son necesariamente en php, si no una serie de metodologías, tecnologías y buenas practicas que antes no se usaban mucho, pero que tienen cada día mas penetración en nuestro mundo.

Pero esto para otro día.

## Conclusion

Podemos decir, entonces sin miedo a equivocarnos que PHP es un lenguaje maduro, con una comunidad muy grande y activa, que se preocupa de hacer las cosas bien, y que no tenemos nada que envidiar a otras comunidades que se supone que son mas cool, pero que de las que no tenemos nada que envidiar.
