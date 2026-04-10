---
title: "Entrevista con Fabien Potencier."
date: 2011-02-18
tags: [php, symfony]
source: https://desarrolla2.com/post/entrevista-con-fabien-potencier
---

# [Entrevista con Fabien Potencier.](https://desarrolla2.com/post/entrevista-con-fabien-potencier)

La entrevista se realizó el siete de enero.

En esta entrevista no entre en detalles técnicos aparte de mencionar componentes y su uso así como los patrones de diseño usados para lograr ciertos aspectos de calidad.

En esta entrevista en cambio me centro en el valor de negocio de Symfony2 y en las razones a favor o en contra de usarlo.

La pregunta principal era: ¿Por qué debería yo usar Symfon2? Asumiendo que frases como "técnicamente mejor" o "mayor calidad" se puedan poner en cifras cuantificables en las que se puede basar una decisión económica.

E: Symfony fue tu primer proyecto PHP. ¿Que parte de el te sirvió de motivación para comenzar desde cero?

FP: Symfony 1 es un poco lento, así que necesitaba encontrar la forma de arreglar esto. Además las "buenas prácticas" han evolucionado mucho durante los últimos cinco años, así que para implementar mi punto de vista era necesario comenzar desde cero.

La gente no se daba cuenta de que yo gastaba horas y días intentando optimizar symfony1 tanto como fuera posible. Pero la arquitectura de bajo nivel no era adecuada, así que no era posible realmente optimizarlo aún más de lo que esta.

E: ¿Así que Symfony1 fue en una dirección en que algunas de las partes eran imposibles de mejorar sin romper la compatibilidad hacia atrás?

FP: ¡Exacto!. También porque yo no escribí todo symfony1. Es un conglomerado entre varios proyectos.

Symfony1 comenzó su vida como un conglomerado entre Mojavi y Propel.

Además yo quería tener un framework fácil de aprender para los recién llegados y Symfony1 tiene demasiados conceptos, así que menos conceptos, más flexibilidad y por supuesto, inyección de dependencias.

Hay otra razón PHP 5.3 y los namespaces esto rompe la compatibilidad hacia atrás.

E: Pero PHP 5.3 es compatible hacia atrás, ¿No?

FP: Seguro, pero si tu quieres usar los "namespaces", necesitas renombrar todas tus clases, así que esto rompe la compatibilidad hacia atrás de las librerías no del lenguaje.

E: En diversas conferencias prometiste "menos conceptos" y menos "WTF" en Symfony2 ¿Como logras esto?

FP: Ahora tengo más experiencia que hace 5 años, he aprendido mucho. Tanto por el desarrollo del framework como porque tengo un montón de feedback, así que se lo que funciona y que lo que no. Menos conceptos encontrando conceptos que son capaces de sustituir otros conceptos viejos.

Este es el caso de DIC (dependency injection container), por ejemplo. El reemplaza diferentes partes de Symfony1: Factorías, config.yml, sfContext, ...

Otro ejemplo es la capa de la vista con templates, layouts, slots, components, component slots, partials, etc. En Symfony2 nosotros solo tenemos templates, layouts y blocks. He aprendido que los nombres son fundamentales, por eso nosotros tenemos un Kernel, un Firewall y así sucesivamente. Me deshice del patrón Singleton, no hay ni un solo Singleton en symfony2.

E: Y no más sfUser?

PF: Exacto!

E: ¿Qué pasa con el sub framework de formularios? Ya fue reescrito en la 1.2

FP: Si miras de cerca el framework de formularios en Symfony2 es solo una evolución del que tenías en Symfony 1, pero la mayoría de los fallos han sido reparados ... esperemos ..

E: PHP ha visto el surgimiento y caída de numerosos frameworks en los últimos años, pero solo unos pocos incluyendo symfony 1.x han podido conseguir una masa crítica. Teniendo en cuenta que Symfony2 ha sido reescrito completamente y no mantiene compatibilidad hacia atrás. ¿Es justo decir que es otro framework?

FP: Sí y no, la filosofía es la misma. Así que si te gusto Symfony1, amarás Symfony2. La filosofía de buenas prácticas como entornos, debug toolbar, framework "todo en uno", admin generator, ... Así que si elegiste Symfony1 por buenas razones, probablemente elegirás Symfony2, Todo se encuentra en un nivel superior en Symfony2.

Por ejemplo la web debug toolbar es mucho mejor en Symfony2, con un completo profiler e interfaz web.

Por supuesto nosotros todavía tenemos fuertes discusiones sobre cosas como la elección de Twig como motor de plantillas. La gente cree que nosotros tomamos decisiones por ellos y la buena noticia es que en Symfony2, puedes cambiarlo todo. Los valores por defectos son coherentes y sensatos, pero eres libre de cambiarlos cuando quieras.

E: Yo he seguido la discusión sobre poner Twig como valor por defecto a la vez que estaba claro que teméis perder a la "vieja" comunidad.

FP: Por supuesto que lo estaba. Pero nosotros no hemos teníamos elección. La web evoluciona rápidamente, si nosotros no seguimos estos cambios, estamos muertos. Este es el camino. Si eres un Desarrollador web, debes estar preparado para aprender cosas nuevas cada día. Debes saber que tendrás que cambiar tus hábitos frecuentemente. SI piensas que aprendiendo una tecnología estarás seguro para los próximos cinco años, estas equivocado y deberías cambiar de trabajo.

E: En tu opinión cual es la característica principal de Symfony2?¿Que hace que destaque?

FP: Simplicidad, o mejor estándares. Nosotros usamos herramientas estándares, como por ejemplo PHPUnit. Simplicidad más estándares igual a HTTP. Nosotros abrazamos HTTP. por eso nosotros no tenemos ninguna capa de cache en las plantillas, usamos HTTP cache en su lugar y esta simplicidad, significa que nosotros somos muy escalables. Flexibilidad es la otra.

E: ¿Cuáles considerarías como características principales de Symfony2 con respecto al resto de los frameworks en el mercado?

FP: Soporte HTTP cache es el único que yo conozco. Dependency Injection Container es también el único. Soporte para un auténtico" sistema de plantillas como Twig es también el único. (todos los demás grandes usan PHP). El sistema de bundles también es único. Hay un montón de cosas.

E: ¿Puede el proceso de cambiar de una versión temprana de Symfony1 a Symfony2 ser considerado como el esfuerzo de cambiar a cualquier otro framework en PHP?

FP: Si y no. Depende, si en tu proyecto sigues buenasprácticass, ( el modelo es independiente por ejemplo ), debería ser relativamente fácil actualizarse. Pero eso no es todo, tú no necesitas actualizar solo por el hecho de actualizar. Symfony1.4 será mantenido por dos años y estoy convencido que la gente invertirá dinero, para actualizar por todas las cosas buenas que tenemos en Symfony2.

E: Por supuesto que quedan dos años, pero si Symfony1 es parte de la estrategia de la compañía, los proyectos existentes no necesitan actualizar, pero los nuevos proyectos merecen el estado del arte de la tecnología.

FP: Exacto!

E: Tú ya has mencionado los beneficios técnicos de usar Symfony2. Pero en mi opinión los beneficios clave de cualquier framework es mejorar los procesos de producción ¿Como se aplica esto en Symfony2?

FP: Por supuesto yo no he hablado de los beneficios "obvios" de usar un framework. Yo pienso que Symfony2 es bueno mejorando tu productividad porque es simple. No hay magia involucrada, no como algunos frameworks donde el 80% de las características son fácilmente implementables pero el 20% restantes son difíciles de lograr.

También porque nosotros seguimos las mejores prácticas. No más necesitar aprender otra librería de test unitarios, nosotros usamos el estándar de facto, PHPUnit.

No necesitas aprender una capa propietaria de cache, nosotros usamos las cabeceras de HTTP planas.

Y por supuesto todas las herramientas que proporcionan ayuda, como el profiler, la web debug toolbar, los entornos, 

Y por último pero no menos importante: La estructura que proporcionamos por defecto. Nosotros proporcionamos por defecto una estructura para tu proyecto, así que no necesitas inventar nada aquí, como en Zend Framework. Así que cualquiera puede ser añadido al equipo fácilmente, el debe encontrarse como en casa y ser productivo desde el día uno.

E: ¿Que otros estándares abiertos hay en Symfony2 aparte de HTTP y PHPUnit?

FP: MVC probablemente, incluso no estoy preocupado por si seguimos el patrón MVC. Quiero decir, MVC no era un patrón web. La separación de capas pienso que es lo importante. También la configuración en XML con XSD o PHP. Yo he aprendido en los últimos años que la separación de capas es probablemente la cosa más importante de seguir el paradigma, ¿No crees?

FP: Si, si tu entiendes SoC (separación de capas) no necesitas realmente otros patrones. Quiero decir, la mayoría de patrones son sencillos de aprender si entiendes la separación de capas.

E: Yo he escrito un post sobre preparar un tu proyecto Symfony1 para Symfony2 a mediados del año pasado yo proponía que la mejor manera de hacer esto es siguiendo SoC tanto como sea posible.

FP: Cierto. La Inyección de dependencias debería ser muy natural cuando tú entiendes la SoC por ejemplo.

Hablando de estándares, nosotros usamos CSS3 para los test funcionales por ejemplo y la ICU para la internacionalización.

Twig también sigue algún tipo de estándares por ejemplo tiene la misma sintaxis que Jinja y Django. Cuando algo que ya existeestáa muy extendido nosotros intentamos aprender de él y adaptarlo al mundo de PHP.

E: ¿Así que los grandes diseños pueden trabajar de forma independiente de la tecnología?.

FP: Si, este es uno de los grandes puntos de Symfony2 y algo que hacemos mucho en Sensio. Los diseñadores web serán capaces de crear las plantillas Twig con variables "fakes", integrarlo con Symfony2 es cuestión de mover los ficheros porque nosotros abstraemos las variables. Por ejemplo {{foo.bar}} es $foo['bar'] para los diseñadores web ( el trabaja con datos auxiliares ) y $foo->getBar() cuando es movido al proyecto Symfony2.

Esto es realmente poderoso, y significa que ellos pueden trabajar en la misma plantilla de forma colaborativa. Antes el diseñador web trabajaba en HTML, después el desarrollador lo convierte a PHP y si hay modificaciones, las hará el desarrollador.

Esto es todo. Todo es más natural y fluido. Esto significa que el desarrollador puede testear cosas. Por ejemplo cuando el tiene algo como {% if user.authenticated %} en una plantilla es muy fácil comprobar el HTML renderizado cuando el usuario está autenticado o no.

No necesitas código PHP. {% set user = {authenticated: true } %} y listo

¿Así que un desarrollo guiado por diseños aprobados es posible cuando el entorno lo define, y después implementar la lógica.?

FP: Correcto. Cuando yo hablo de flexibilidad, lo digo en serio.

E: ¿Es así como trabajáis en Sensio?

FP: Si, esta es la forma en la que nosotros trabajamos en Sensio y así ahorramos un montón de tiempo, haciendo todo más fluido y teniendo menos problemas. Yo encontré grandes ejemplos de documentación en los últimos años como "Jquery para diseñadores". ¿Podría pasar algo similar con Symfony2?

FP: Yo no he leído ese documento, así que no te puedo decir, pero si, yo tengo en mi lista de tareas un ítem sobre escribir documentación para diseñadores web y la interacción con los desarrolladores.

E: Otra pregunta en la dirección de los beneficios de la compañía. ¿Cómo puede una compañía beneficiarse de elegir Symfony2 después del desarrollo de la aplicación en términos de calidad y mantenibilidad? ¿Son posibles ambos efectos positivos usando Symfony2?

FP: Un framework no puede dar la calidad. Esto es responsabilidad del desarrollador, pero si nosotros proporcionamos todas las herramientas y las mejores practicas para ayudar al desarrollador como integración con test unitarios, integración con test funcionales, ( de forma mucho mejor que en Symfony1 ), la estructura por defecto para los proyectos, convenciones de nombres, bundles ...

Nosotros no hemos hablado de los bundles, pero es algo de lo que yo también estoy orgulloso. Todo es un bundle en Symfony 2. No solo el código que tú escribes, el núcleo del framework y el código de terceros.

Esto significa que no hay diferencia entre un código escrito para tu aplicación y el código que tu quieres compartir con otros. En cualquier momento puedes compartir código entre proyectos. Un bundle es un concepto muy simple ya que es solo un espacio de nombres bajo el que se guardan tus paquetes.

Hay algunas convecciones por supuesto que puedes seguir para simplificarte la vida un montón. Symfony1 fue exitoso porque tenía plugins.

Los bundles son plugins con esteroides.

E ¿Puedes explicar la parte de los esteroides un poco más?

FP: No hay una sola idea tardía como en Symfony1.

Todas están en el núcleo de Symfony2.. El kernel necesita bundles para ser algo usable.

E: ¿Cómo se Beneficia el desarrollador más de lo que lo hacía en Symfony1?

FP: SoC de nuevo. En Symfony1, esto fue añadido tarde al juego, así que había cosas que no eran posibles. Tan pronto como añadías algunos plugins en Symfony1, tú podrías sufrir problemas de rendimiento, esto es menos probable en Symfony2.

Tú puedes felizmente tener algunos bundles y Symfony2 hace frente a eso.

Por la integración en el núcleo y gracias al DIC ( todo es compilado durante la primera petición ).

No hay código necesario para arrancar un bundle.

E: ¿Como consideras de empinada la curva de aprendizaje de Symfony2 y cuales son las cosas esenciales?

FP: Symfony 2 debería ser realmente fácil para los recién llegados. Solo necesitan aprender PHP, y OOP pienso que eso es todo. Si echas un vistazo, verás que no hablamos sobre MVC u otros conceptos en el "quick tour" porque no lo necesitamos.

Tampoco hay que configurar nada. En el "quick tour" no habla de configuración. Escribir unos pocos controladores y plantillas es realmente fácil en Symfony2.

Por supuesto usar Doctrine 2 o el framework de formulario es más difícil, pero las cosas básicas son realmente fáciles. Con Symfony1 tampoco hay que configurar nada.

El problema es la documentación. Porque el libro dedica muchas páginas a la configuración, la gente pensó que este paso era obligatorio. No lo es, además de la configuración de la base de datos por supuesto y el routing.

La configuración es mucho más poderosa en Symfony2 ya que tenemos lo que se llama configuración semántica. La composición en cascada se hace en tu código no por el framework.

Esto era el mayor problema de Symfony1. La rutas son mucho más simples que en Symfony1.

Aquí hay SoC de nuevo.

En Symfony2, las rutas hacen solo una cosa, convertir la información de la ruta en parámetros y eso es todo.

No necesitas crear objetos de rutas. La características cubiertas por los objetos de ruta siguen ahí pero hechas por otro subframework mucho más poderoso. Se llama el ParamConverter.

Teniendo una función showAction($id) se le inyecta un objeto directamente function showAction(Post $post).

Esto es más flexible, rápido y fácil de comprender y más poderoso ( y con menos código )

Las anotaciones son también un tema importante, sobre todo para los recién llegados. El hace las cosas mucho más simple para ellos. Ellos pueden hacer todo en el controlador sin tener que abrir otros ficheros, (como en Symfony1) Aquí un pequeño ejemplo:

```
`/**
* @Route("/:id")
* @ParamConverter("post", class="BlogBundle:Post")
* @Template("BlogBundle:Annot:post", vars={"post"})
* @Cache(smaxage="15")
*/
public function showAction(Post $post)
{
  // ..
}
`
```

(Este ejemplo está obsoleto, pero muestra el concepto) Y hay lo mismo para el ORM y la configuración de validaciones.

Tú no necesitas leer la documentación para entender lo que hace y ese es el punto. Tu modelo es solo parte de las clases de PHP, así que una clase usuario podría ser.

```
`class User {...}`
```

Entonces si quieres serializarlo con el ORM y/o el ODM tu solo necesitas añadir anotaciones

```
`
/**
* @orm:Entity
*/

class User
{
  /**
  * @orm:Id @orm:Column(type="integer")
  * @orm:GeneratedValue(strategy="AUTO")
  */

  private $id;
}`
```

O con la validación

```
`
/**
* @orm:Column(type="string", length=50)
* @validation:NotBlank
* @validation:MinLength(4)
*/
}`
```

Y por supuesto, puedes hacer lo mismo con los ficheros de configuración. ¿Quieres declarar esta clase como la clase del Usuario?

```
`class User implements AccountInterface`
```

Cualquier clase puede ser usada como clase de Usuario en Symfony2, esto es mucho mejor que el sfGuardPlugin, donde te obliga a adoptar el mismo esquema.

E: Déjame continuar un poco más. Con independencia de componentes como el routing el nuevo sistema de bundles, la nueva cache, la nuevas mejoras de seguridad, el nuevo framework de formularios y el admin generator

¿Cómo puedes lograr que los usuarios se sientan cómodos con todo esto? Pueden todos estos componentes y subframewors sentirse como una única tecnología.

FP: Por supuesto que lo es. Tú puedes usar el framework full-stack, y la mayoría de la gente lo hará. Pero la buena noticia es que tú puedes usar componentes separados y todo desde una interfaz unificada. Todo es posible gracias a al DIC. Por esto no es importante si usan Zend Logger en Symfony2 o porque la integración con Switmailer y Twig es así de Natural. Por que todo es manejado por el DIC. Toda la configuración de estos componentes ( desde symfony a las partes de terceros ) se oculta bajo la configuración semántica que ofrecemos.

Algunas personas podrían tener miedo a que Symfony2 se convierta en el próximo Zend Framework, que es más una librería que un framework, además de poco homogéneo

FP: No deben tenerlo, esto esta lejos de la verdad. Los componentes que tenemos sirven para un framework full-stack. Desacoplarlos es posible, esto no significa que necesites usarlos desacoplados.

Ellos son todavía coherentes, gracias al DIC esto es todo lo contrario.

Por ejemplo aquí tienes como configurar el Zend Logger.

```
`zend.config:
otranslator:
olocale: en
oadapter: Zend\Translator\Adapter\ArrayAdapter`
```

Y Doctrine DBAL

```
`doctrine.dbal:
odbname: sfweb
ouser: root
opassword: ~
`
```

Y Swiftmailer:

```
`swift.mailer:
otransport: smtp
oencryption: ssl
oauth_mode: login
ohost: smtp.gmail.com
ousername: Fabien Potencier
opassword: xxxxxxxx`
```

Como ves ellos se configuran igual. Como si todas estas librerías fueran componentes de Symfony2, pero por supuesto no lo son. Lo que tambien demuestra la naturaleza semantica de la configuración.

FP: Exacto. Nosotros escondemos las diferencias y mostramos una configuración unificada.

E: Déjame ser un poco provocativo. ¿No pueden ser las anotaciones y el DIC considerados mágicos?

FP: Las anotaciones si, el DIC NO! No hay nada mágico en el DIC, todo lo contrario. Solo mira el código "compilado" y verás que el código es exactamente igual que el código que escribirías a mano.

Excepto que esta optimizado, y solo se invoca bajo demanda.

La mayoría de los desarrolladores nunca trabajarán con las partes internas de DIC. Las anotaciones son algo más mágicas, pero esto es opcional. Esto es por que el controlador de ejemplo que te mostré antes solo funciona con el FrameworkExtraBundle, las anotaciones son solo una forma de configurar cosas. Tú puedes definir, por ejemplo el ParamConvertes de una forma menos mágica que con las anotaciones?

FP: Si no te gusta, no lo uses, tu puedes configurarlo con un XML plano (No disponible todavía).

E: ¿Qué partes de Symfony2 pueden ser reconocidas por los usuarios familiarizados con Symfony 1?

FP: Entornos, web debug toolbar, routing, formularios ( pero mejor ), organización en bundles/plugins, y lo mas importante, la filosofía es exactamente la misma. La actualización de Symfony1.0 a 1.1 fue dolorosa por decir algo. y la actualización a 2.0 podría no ser posible.

E: Cuando una cierta release es importante para las compañías. ¿como Symfony prepara el futuro road map?

FP: El objetivo es proponer una API estable que no cambia entre las versiones menores (2.0 -> 2.n), así que si tu solo usas esta API, estas a salvo. La API será probablemente pequeña para 2.0 y nosotros iremos añadiendo mas cosas que podamos estabilizar. Así que cada versión, añadirá mas cosas a la API estable. También, la flexibilidad podría permitirnos mejor compatibilidad hacia atrás como podría facilitarnos capas de compatibilidad sin sacrificar el rendimiento.

E: ¿Gracias al DIC?

FP: Exactamente. Yo no quiero usar el DIC en todos mis argumentos, pero la realidad es que el DIC nos ayudó un montón.

E: Yo tengo la impresión de que la comunidad de Symfony a ganado mucho más impulso con esta nueva versión. ¿Como ves la participación de la comunidad en comparación con las versiones anteriores?

FP: Esto es insano. Nosotros tenemos mucha gente contribuyendo, y muchos muy buenos desarrolladores, esto es increíble. Yo pienso que yo he combinado más de 50 peticiones de pull en los últimos dos días, de más de 20 personas diferentes.

Estoy orgulloso de ellos, Esto hace que yo esté realmente feliz y Git es probablemente parte de este éxito. Estoy seguro que de la comunidad crecerá más rápido con Symfony2. Incluso si el crecimiento ha sido bueno en los últimos 5 años,siento que algo especial esta a punto de ocurrir.

E: ¿Esperas que la comunidad abandone pronto Symfony1 en favor de Symfony2?

FP: Estoy realmente sorprendido por el flujo de hilos relacionados con Symfony2 en la lista de correo. Hay que mucha gente está ya usando Symfony2 en producción, están locos.

Pienso que las empresas deben trabajar con Symfony1 por un poco más de tiempo, pero pequeñas empresas, estudiantes, desarrolladores freelance, empresas ágiles pueden cambiar de la noche a la mañana.

De hecho algunas ya han cambiado!

E: A la larga todas las empresas también lo están.

FP: Yo espero que la comunidad de Symfony2 supere a la comunidad de Symfony1 antes de Junio de 2011

E: Que es el momento en que se acaba el soporte a Doctrine 1.2.

FP: Correcto.

E: Última pregunta. ¿Symfony2 será lanzado en la Symfony Live 2011?

FP: El plan es lanzar Symfony2 durante el hacking day de la Symfony Live de parís y yo pienso que esto es posible, difícil, pero no imposible.

E: Recuerdo que dijiste algo similar hace algún tiempo

FP: Por supuesto, si nosotros pensábamos que Symfony2 no está suficientemente estable o si algunas decisiones de diseño, nos hace cambiar demasiadas cosas nosotros pospondremos la versión.

Pero a día de hoy, en confianza nosotros creemos que es posible que esté.

La parte más difícil puede ser decir no a mejoras y añadirlas para la 2.1, frustrara un montón a las personas que han contribuido a Symfony2. La Beta 1 se espera que sea liberada la próxima semana, por lo que seremos muy cuidadosos de no hacer grandes cambios.

En el momento de publicar este artículo la Beta 1 ya ha sido liberada.

Podéis encontrar el artículo completo en http://test.ical.ly/2011/02/16/an-interview-with-fabien-potencier/
