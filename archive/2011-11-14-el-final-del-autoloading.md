---
title: "El final del autoloading"
date: 2011-11-14
tags: [php]
source: https://desarrolla2.com/post/el-final-del-autoloading
---

# [El final del autoloading](https://desarrolla2.com/post/el-final-del-autoloading)

El autoloading en PHP ahorra mucho tiempo. Nos permite escribir scripts sin conocer exactamente la estrura de directorios que utilizan nuestras librerías. Pero con la llegada de los namespaces en PHP5.3 y la influencia de Java en la nueva generación de frameworks PHP, el autoloading está cambiando. En un futuro cercano el autoloading explicito puede estar por todas partes, pero sin las ventajas del viejo estilo.

## Antes del autoloading, había rutas de ficheros

Antes del autoloading, en cada fichero de clase debía ser explicitamente declarado los paths a sus dependencias. El código fuente era parecido al ejemplo, tomado de la librería PEAR.

```
<?php
require_once 'PEAR.php';
require_once 'PEAR/DependencyDB.php';

class PEAR_Registry extends PEAR {
 //...
}
```

Las dependencias aparecen al principio de cada clase. En este fragmento de código, incluso si la primera vez que se utiliza la clase PEAR_DependencyDB está escondido en la línea 328 de PEAR_Registry class. La dependencia es muy fuerte. En la mayoría de los casos la ruta es relativa y el interprete de php tiene que revisar toda las opciones incluidas en el include_path. El rendimiento disminuye en tanto que el tamaño de include_path aumenta. La parte superior de muchos ficheros esta recargada con llamadas require_once que disminiyen la legibilidad.

## El autoloading de la librería estandar de php ( SPL )

require_once era lento. En servidores con acceso a disco lento o sin cache opcode , era mejor no usar require_once. La funcion de PHP sp_autoload_register() comenzó a usarse. Hizo posible remover todas las llamadas a require_once del código. Y hacía las aplicaciones más rápidas. Pero el mayor beneficio era que tu podrías usar clases sin conocer donde está el fichero de código fuente dentro de la estructura de directorios. El ejemplo es un extracto del tutorial "My First Project" en symfony.

```
<?php
class postActions extends sfActions
{
 public function executeList()
 {
    $this->posts = PostPeer::doSelect(new Criteria());
 }
}
```

Hay lo tienes, no hay ningún require_once, incluso aunque esta clase depende de las clases sfActions, PostPeer y Criteria. Los desarrolladores pueden centrarse en la lógica de negocio, sin malgastar un segundo buscando donde se encuentran las dependencias. Así es como funciona el desarrollo rápido de aplicaciones.

## Implementaciones de autoloading

La implementación actual del autoloading varía, algunas librerías como Propel, incluyen una lista de todas las clases que pueden ser requeridas, junto con los paths a los ficheros de código fuente. En el ejemplo un extracto de la clase Propel

```
<?php
class Propel
{
 // ..
 protected static $autoloadMap = array(
    'DBAdapter'      => 'adapter/DBAdapter.php',
    'DBMSSQL'        => 'adapter/DBMSSQL.php',
    'MssqlPropelPDO' => 'adapter/MSSQL/MssqlPropelPDO.php',
    'MssqlDebugPDO'  => 'adapter/MSSQL/MssqlDebugPDO.php',
    // etc.
}
```

Está técnica permite esconder las rutas a las clases, pero obliga al desarrollador de la librería a mantener actualizado el mapa de clases y rutas cada vez que introduce una nueva clase. Otra técnica como la que utiliza symfony framework ( NT: en la version 1.x ) utiliza una clase "file-iterator" que busca en el árbol de directorios del proyecto todos los ficheros ".class.php" y los indexa. A pesar de que tiene un impacto en la primera petición, esta técnica elimina el trabajo de mantener el mapa de clases y rutas, y también funciona para las clases que no pertenecen al framework. Además, la técnica de autoloading utilizada en symfony permite sobreescribir las clases que contiene el framerwork por las propias. El "file-iterator" busca en el árbol de directorio en un determinado orden, los directorios del usuario, los directorios del proyecto, los de los plugins y por último los directorios del framework. Así que el desarrollador puede crear una clase PostPeer que será utilizada en lugar de la clase PostPeer que contiene un plugin. El autoloading estaba de moda, rápido, potente y conciso.

## Autoloading basado en Namespaces

La llegada de los namespaces en PHP 5.3 obligo a cambiar las técnicas de autoloading. Una iniciativa comenzó con los autores de algunos frameworks que trataron de definir una técnica que permitiera la interoperabilidad entre sus librerías y sus implementaciones de autoloading. Se llamarón el PHP Standards Working Group, esta comunidad acordó que es mejor explicito que implícito, y que un nombre de clase completo sería una ruta relativa al fichero fuente de su clase.

```
\Doctrine\Common\IsolatedClassLoader  => /path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php
\Symfony\Core\Request  => /path/to/project/lib/vendor/Symfony/Core/Request.php
\Zend\Acl  => /path/to/project/lib/vendor/Zend/Acl.php
\Zend\Mail\Message  => /path/to/project/lib/vendor/Zend/Mail/Message.php
```

Las librerías de acuerdo con la iniciativa deberán seguir unos principios en la estructura de nombres de ficheros y directorios, y facilitar una implementación de autoloading compatible con el ejemplo de la clase SplClassLoader. Ese es el caso de la mayoría de frameworks de nueva generación en 2011. Por ejemplo un extracto de el nuevo My First Project en symfony2.

```
<?php
namespace Application\HelloBundle\Controller;
use Symfony\Framework\WebBundle\Controller;

class HelloController extends Controller
{
 public function indexAction($name)
 {
    $author = new \Application\HelloBundle\Model\Author();
    $author->setFirstName($name);
    $author->save();
    return $this->render('HelloBundle:Hello:index', array('name' => $name, 'author' => $author));
 }
}
```

Tampoco existen requiere_once en este código, el sistema de autoloading se encarga de ello. El autoloading de PHP busca la clase Symfony\Framework\WebBundle\Controller en el fichero Symfony/Framework/WebBundle/Controller.php. La ruta al fichero no está relacionada con el include_path, ya que el autoloader debe ser inicializado con la ruta base al directorio de la librería. La primera ventaja es que no hay penalización al rendimiento en la primera petición. También que las dependencias son explicitas de nuevo. Finalmente si tu quieres sobre escribir una clase facilitada por el framework puedes hacerlo fácilmente como se ve en el ejemplo

```
<?php
namespace Application\HelloBundle\Controller;
// use a custom Controller class instead of the framework's Controller
use Application\HelloBundle\Tools\Controller;

class HelloController extends Controller
{
 // same code as before
}
```

## Se acabo el desarrollo rápido de aplicaciones.

¿No te recuerda algo del ejemplo anterior?, Cierto, es muy similar a las llamadas require_once del primer ejemplo cuando aún no teníamos autoloading.

```
< ?php
// old style
require_once 'Application/HelloBundle/Tools/Controller.php';
// new style
use 'Application\HelloBundle\Tools\Controller'
```

El nivel de detalle que añade el autoloading basado en namespaces se complensa con la facilidad de uso introducido por la SPL ( standar php library ) autoloading colocada al comienzo del fichero. El problema es no solo tener que escribir más código. Fijate en el trabahjo que tiene usar una clase de un framework de nueva generación. Evaluar la estructura de directorios del framework, buscando el fichero fuente de la clase que necesita. Abril el fichero fuente, y copiar la declaración de espacios de nombres Copiar la declaración de espacio de nombres en sentencias "use" de tu clase. Esta tarea de copiar y pegar ocupa un monton de tiempo cuando trabajas con Symfony2, por ejemplo.  Esto puede ser de alguna manera subsanado si utilizas un IDE con auto completado de código, pero aún es necesario conocer los nombres completos de las clases que necesitas. Es necesario conocer bien las clases del framework para usarlo. Esto es un paso hacia atras en terminos de usabilidad comparado con el autoloading de los frameworks de primera generación, conocer el nombre de las clases era suficiente.

## No hay nada mejor en PHP

¿No sería genial utilizar los frameworks de nueva generación sin conocer donde están las dependencias en el sistema de ficheros?. Podrías escribir código de un controlador de Symfony2 como este:

```
< ?php
class HelloController extends Controller
{
 public function indexAction($name)
 {
    $author = new Author();
    $author->setFirstName($name);
    $author->save();
    return $this->render('HelloBundle:Hello:index', array('name' => $name, 'author' => $author));
 }
}
```

Un pequeño autoloader podría capturar la llamada a la clase controlador, abrir la implementación por defecto ( en Symfony/Framework/WebBundle/Controller.php ) y dinamicamente vincular Symfony\Framework\WebBundle\Controller con Controller. Salvo que PHP, la vinculación se realiza en tiempo de compilación, por lo que no funcionaría. Existe una posibilidad de implementar este autoloader utilizando eval(), pero esto probablemente sea peor que realizar los require a mano. Además, vincular todas las clases en una capa de usabilidad en una capa superior del framework no es posible tampoco. Podría fallar en los nombres de clase duplicados. (ej. Symfony\Framework\WebBundle\Command and Symfony\Components\Console\Command\Command). Amenos que los autores de frameworks cambien su forma de pensar sobre el autoloading, el futuro de PHP será detallado.

## Resolviendo el problema.

Yo pienso que añadir la carga de clases detallada disminuye la velocidad de desarrollos. mira los microframeworks por ejemplo. Te ofrecen una forma de responder a una petición HTTP rápidamente con una separación minima MVC. Compara el código de "Hello, World" escrito en Slim, un microframework sin autoloading basado en namespaces y Silex un microframework que si lo utliza.

```
<?php
// Hello world with Slim
require_once 'slim/Slim.php';
Slim::init();
Slim::get('/hello/:name', function($name) {
    Slim::render('hello.php', array('name' => $name));
});
Slim::run();

// Hello world with Silex
require_once 'silex.phar';
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Templating\Engine;
use Symfony\Component\Templating\Loader\FilesystemLoader;
use Silex\Framework;
$framework = new Framework(array(
 'GET /hello/:name' => function($name) {
    $loader = new FilesystemLoader('views/%name%.php');
    $view = new Engine($loader);
    return new Response($view->render(
     'hello',
     array('name' => $name)
    ));
 }
));
$framework->handle()->send();
```

En el segundo ejemplo, el autoloading es explicito y lo hace un poco más dificil. Los desarrolladores de frameworks de nueva generación explica que añadir el detalle explicito es el precio a pagar por una mejor calidad de código. Yo no estoy de acuerdo. Yo no quiero ver PHP como el próximo java donde el código es bueno desde el punto de vista de procesado,, pero muy costoso de escribir. Me hace querer cambiar a otros lenguajes donde el autoloading basado en namespaces es una discusión que nunca tuvo lugar y donde el desarrollo rápido de aplicaciones es algo que todavía es posible. Ruby por ejemplo. El ofrece un microframework llamado Sinatra que hace "Hello, world" muy conciso.

```
require 'sinatra'
require 'erb'
get '/hello/:name' do |name|
    @name = name
    erb :hello
end
```

Mira, hay un require en este script, y todavía es muy rápido y fácil de usar. Traducción libre de: [http://propel.posterous.com/the-end-of-autoloading](http://propel.posterous.com/the-end-of-autoloading)
