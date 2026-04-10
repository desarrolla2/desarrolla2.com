---
title: "La especificación HTTP y los fundamentos de Symfony 2"
date: 2011-03-07
tags: [php, symfony]
source: https://desarrolla2.com/post/la-especificacion-http-y-los-fundamentos-de-symfony-2
---

# [La especificación HTTP y los fundamentos de Symfony 2](https://desarrolla2.com/post/la-especificacion-http-y-los-fundamentos-de-symfony-2)

Antes avanzar más con Symfony 2, vamos a empezar por comentar el Protocolo de transferencia de Hiper Texto ( HTTP ) un formato comunicación simple usado por todos los clientes y servidores cuando se comunican unos con otros. Es importante por que como descubriremos, El núcleo de Symfony 2 ha sido diseñado para usar HTTP, no para reinventar nada. El producto final es un framework diferente a los demás, que realiza una abstracción sobre las reglas fundamentales de la World Wide Web. Conscientemente o no, HTTP es algo que usamos todos los días. Symfony 2, te permitirá aprovecharlo y dominarlo.

## El cliente envía la petición

La web está construida sobre la idea de que cada comunicación comienza cuando un cliente realiza una petición a un servidor. La petición es un simple mensaje de texto creado por el cliente en un formato especial conocido como petición HTTP. Hay muchos tipos de clientes, navegadores web, servicios web, clientes de RSS, cada uno envía una petición en un formato básico como por ejemplo:

```
GET /index.html HTTP/1.1
Host: www.example.com
Accept: text/html
User-Agent: Mozilla/5.0 (Linux; X11)

```

La primera línea de una petición HTTP es la más importante ( y de hecho es la única obligatoria ). Contiene dos cosas, la URI y el método HTTP. La URI ( URL si contiene la cabecera Host ) identifica de forma única la localización de un recurso mientras que el método HTTP define que es lo que quieres hacer con el recurso. En el ejemplo, la localización única del recurso es /index.html y el método HTTP es GET. En otras palabras, el cliente solicita recuperar el recurso identificado por /index.html

Los métodos disponibles en una petición HTTP definen las pocas maneras en la que podemos actuar sobre un recurso:

- **GET**: recupera un recurso del servidor
- **POST**: Crea un recurso en el servidor
- **PUT**: Actualiza un recurso en el servidor
- **DELETE**: Elimina un recurso del servidor

Sabiendo esto, podemos imaginar aspecto debe tener una petición HTTP para borrar un recurso especifico, por ejemplo de la entrada de un blog:

```
DELETE /blog/15 HTTP/1.1

```

Actualmente hay definidos nueve métodos HTTP en las especificaciones HTTP, pero la mayoría de ellos no están soportados ampliamente. En realidad, la mayoría de los navegadores modernos no soportan siquiera los métodos PUT y DELETE. Una cabecera adicional que si esta ampliamente soportada, es el método HEAD, que pide la respuesta de un método GET, pero sin el cuerpo de la respuesta.

Adicionalmente a la primera línea, una petición HTTP normalmente contiene otras líneas de información conocidas como cabeceras de la petición HTTP. Estas cabeceras, ofrecen una amplia gama de información adicional, como el HOST solicitado, los formatos que el cliente acepta como respuesta (Accept) y la aplicación que se está usando para hacer la petición (User-Agent). Existen otras cabeceras y puedes encontrarlas en el articulo de la [lista de campos de las cabeceras HTTP](http://en.wikipedia.org/wiki/List_of_HTTP_header_fields) en wikipedia.

## El servidor responde

Ahora que el servidor ha leído una petición con formato HTTP del cliente, el sabe exactamente que recurso quiere el cliente identificado por una URI y que quiere hacer con el (método HTTP). En este caso una petición GET, el servidor prepara el recurso y responde con una respuesta HTTP:

```
HTTP/1.1 200 OK
Date: Fri, 12 Nov 2010 12:43:38 GMT
Server: Apache/2.2.14 (Ubuntu)
Connection: Keep-Alive
Content-Length: 563
Content-Type: text/htmlHello Symfony2 World!

```

La respuesta HTTP del servidor al cliente contiene no solo el recurso solicitado ( el contenido HTML en este caso ), si no también otra información sobre la respuesta. Como en la petición HTTP, la primera línea de la respuesta es especialmente importante y contiene el código de estado de la respuesta HTTP ( 200 en este caso ). El código de estado es extremadamente importante y comunica el resultado global de la respuesta al cliente. Hay diferentes códigos para peticiones correctas, peticiones que fallan y peticiones que requieren una acción en el cliente, ( como por ejemplo una redirección ). Una lista completa puede ser descargada de [la lista de códigos de estado HTTP](http://translate.google.es/#en|es|the%20overall%20outcome%20of%20the%20request%20back%20to%20the%20client.) de la wikipedia.

Como en la petición, un mensaje de respuesta HTTP puede contener información adicional. Se les conoce como encabezados HTTP y se encuentran entre la primera línea ( código de estado ) y el contenido de la respuesta.

Una cabecera HTTP importante en la respuesta es el Content-Type. El cuerpo de un mismo recurso puede ser obtenido en diferentes formatos incluyendo HTML, XML o JSON, por decir algunos. La cabecera Content-Type informa al cliente del formato en el que se está respondiendo.

Como sabes existen muchas cabeceras HTTP. Algunas son muy potentes y pueden ser usadas por ejemplo, para gestionar un sistema potente de cache.

## HTTP y la comunicación cliente-servidor

Este intercambio de peticiones y respuestas el es proceso fundamental que impulsa todas las comunicaciones en el World Wide Web. Y es tan importante y potente ya que es un proceso simple. De hecho los servidores realizan comunicaciones cliente-servidor cada vez que enviamos y recibimos mensajes de correo cada día. HTTP es un lenguaje de uso común para estos mensajes que un conjunto heterogéneo de aplicaciones y máquinas usan para comunicarse.

¿Pero por que un libro sobre symfony explica tan profundamente peticiones, respuestas y el formato HTTP?. Independientemente del framework que elijas, el tipo de aplicación que estés construyendo ( web, móvil, API JSON ), o la filosofía de desarrollo que sigas, el objetivo final es que el servidor entienda la petición y cree y devuelva la respuesta apropiada. Symfony está pensado para adaptarse a esta realidad.

Para aprender más sobre las especificaciones HTTP, nosotros recomendamos leer la especificación original **HTTP 1.1 RFC** o el **HTTP Bis**, que es un esfuerzo activo para aclarar la especificación original. Una gran herramienta para comprobar peticiones y respuestas mientras navegamos es la extensión **Live HTTP Headers** para Firefox.

## Peticiones y respuestas en Symfony

En PHP nos encontramos con un array de variables y métodos que permiten al desarrollador acceder a la petición y enviar la respuesta. Para la información de la petición PHP prepara variables super globales como $_SERVER y $_GET. Recordemos que cada petición es solo un bloque HTTP formateado como texto. La transformación del mensaje de petición a variables super globales es realizado por PHP y tu servidor web. Al final el resultado es que el mensaje de la petición está disponible en PHP, pero en una colección de variables diferentes de ámbito súper global.

Como desarrollador orientado a objetos, necesitamos una forma mejor ( orientada a objetos ) de acceder a la información de la petición. Symfony facilita una clase Request que es simplemente una representación orientada a objetos del mensaje de petición HTTP. Con el, tendrás toda la información de la solicitud a tu alcance.

```
use Symfony\Component\HttpFoundation\Request;

$request = Request::createFromGlobals();

// the URI being requested ((e.g. /about) minus any query parameters
$request->getPathInfo();

// retrieve GET and POST variables respectively
$request->query->get('foo');
$request->request->get('bar');

// retrieves an instance of UploadedFile identified by foo
$request->files->get('foo');

$request->getMethod();          // GET, POST, PUT, DELETE, HEAD
$request->getLanguages();       // an array of accepted languages

```

El método getPathInfo() es especialmente importante, ya que devuelve el URI que ha sido solicitado en relación a tu aplicación. Por ejemplo, suponiendo que la aplicación esta siendo ejecutada en el subdirectorio foo del servidor:

```
// http://example.com/foo/index.php/bar
$request->getPathInfo();  // returns "bar"

```

Symfony también proporciona una clase Response, que es una simple abstracción en PHP del mensaje de respuesta HTTP en bruto . Esto permite que tu aplicación utilice una interfaz orientada a objetos para construir la respuesta que será enviada al cliente.

```
use Symfony\Component\HttpFoundation\Response;
$response = new Response();

$response->setContent('
```

# Hello world!

```
');
$response->setStatusCode(200);
$response->headers->set('Content-Type', 'text/html');

// echos the headers followed by the content
$response->send();

```

En este momento, si symfony no hiciera nada más, ya tendrías un framework para acceder a la información de la solicitud y una interfaz orientada a objetos para crear la respuesta. Symfony facilita un amplio conjunto de herramientas, sin ocultar la realidad de que el objetivo final de cualquier aplicación web es procesar una petición HTTP y generar una respuesta HTTP apropiada basada en la lógica de negocio específica de la aplicación. Incluso cuando discutimos las características de symfony, este es fundamental y transparente.

Las clases Request y Response forman parte de un componente independiente incluido en Symfony, que se llama HttpFoundation . Este componente puede ser utilizado de forma independiente y facilita clases para manejar las sesiones y la subida de ficheros.

## Viajando desde la petición a la respuesta

Ya sabemos que el objetivo final de una aplicación es usar la petición HTTP para crear y devolver una respuesta HTTP apropiada. Symfony facilita clases Request y Response que te permite hacerlo con una interfaz orientada a objetos. Hasta ahora, solo estamos aprovechando un pequeño pedazo de Symfony. Pero ya tenemos herramientas para escribir una aplicación sencilla. Vamos!:

```
$request = Request::createFromGlobals();
$path = $request->getPathInfo(); // the URL being requested
$method = $request->getMethod();

if (in_array($path, array('', '/') && $method == 'GET') {
    $response = new Response('Welcome to the homepage.');
} elseif ($path == '/about' && $method == 'GET') {
    $response = new Response('About us');
} else {
    $response = new Response('Page not found.', 404);
}
$response->send();

```

En este simple ejemplo, la aplicación procesa correctamente la petición y devuelve la respuesta apropiada. Desde un multo de vista muy técnico, nuestra aplicación hace exactamente lo que tiene que hacer.

## Una aplicación sin Framework

¿Pero y si la aplicación necesita crecer? Imagina esta misma aplicación si se viera forzada a manejar cientos o incluso miles de páginas diferentes. Con el fin de mantener las cosas escalables ( Por ejemplo no todo en el mismo fichero ), necesitamos reorganizarlo todo. Para empezar, podríamos mover el trabajo de crear la respuesta en un conjunto de funciones diferentes. Estas funciones se conocen comúnmente como controladores y permiten organizar mejor nuestro código.

```
if (in_array($path, array('', '/') && $method == 'GET') {
    $response = main_controller($request);
} elseif ($path == '/about' && $method == 'GET') {
    $response = about_controller($request);
} else {
    $response = error404_controller($request);
}

function main_controller(Request $request)
{
    return new Response('Welcome to the homepage.');
}

function about_controller(Request $request)
{
    return new Response('About us');
}

function error404_controller(Request $request)
{
    return new Response('Page not found.', 404);
}

```

Sigamos, nuestra próxima cada vez mayor aplicación contiene un motón de bloques if-elseif que en rutan la creación de objetos Response desde diferentes controladores ( es decir, un método PHP ). Podríamos considerar una construcción basado en un sistema configurable de enrutamiento que mapee cada petición con un controlador específico basado en la URI y el método HTTP de la petición.

Obvio o no, la aplicación esta comenzando a descontrolarse. Recordemos que el objetivo de cualquier aplicación es utilizar una la lógica personalizada de la aplicación y la información de la petición para crear una respuesta apropiada. En nuestra aplicación, los cambios propuestos no son lógica de negocio. En su lugar la refactorización nos lleva a inventar un sistema de controladores y un sistema de enrutamiento personalizado. A medida que continua el desarrollo, nosotros no podremos enviar algún tiempo de desarrollo en nuestra aplicación y algo de tiempo en la mejora y desarrollo del framework que lo rodea.

Necesitamos una solución mejor, una en la que el desarrollador gaste su tiempo desarrollando la lógica de la aplicación para crear objetos Response en lugar de detalles de bajo nivel.

El framework symfony hace esto, permitiéndote enfocarte en lo que aporta valor, sin sacrificar el poder y la organización de un framework. Por supuesto un framework popular como Symfony tiene una larga lista de "bonos" como estar libre de mantenimiento, documentación, estandarización, y conjunto de bundles open source generados por la comunidad ( Es decir, plugins ) disponibles para usar

Symfony se basa en un núcleo con una única responsabilidad, facilitar el viaje desde el objeto de Request, hasta el objeto final Response. El Núcleo es el que maneja cada petición y de echo ejecuta el código de tu aplicación.

El "código de la aplicación" ejecutado por el Núcleo se denomina "controlador", un termino especial que de hecho es una función PHP ( en la mayoría de los casos, un método de un objeto ). El controlador es donde el código de la aplicación vive, es donde creas el objeto Response final. El núcleo trabaja determinando y llamando al controlador adecuado a cada petición:

```
Request -> Kernel::handle() -> Controller (your code) 
-> Response (returned by controller)

```

Nuestra aplicación de ejemplo anterior debe ser refactorizado en dos "controladores", como en este ejemplo, los métodos PHP pertenecen a la clase myController. El código necesario para determinar y ejecutar estos controladores esta aislado en otro lugar y es gestionado por el Núcleo.

```
class myController
{
    public function homepageAction()
    {
        return new Response('Welcome to the homepage.');
    }

    public function aboutAction()
    {
        return new Response('About us');
    }
}

```

Observe que cada controlador devuelve un objeto Response. Esto es una tarea básica en tus controladores, aplicar la compleja lógica de negocio y construir y devolver en última instancia la respuesta final.

¿Pero como hace el Núcleo para saber que controlador llamar en cada petición? Aunque este proceso es completamente configurable, Symfony 2 integra un "Router" que usa un "mapa" para conectar la información del path de la petición con un controlador específico.

```
Request -> Kernel::handle() -> Controller -> Response
                    |    ^
                    | controller
                    |    |
                    v    |
                    Routing

```

Bien, hablaremos un montón sobre controladores y el Router en los capítulos posteriores

La clase del Núcleo es parte de un componente independiente de Symfony2 llamado HttpKernel. Este componente facilita funcionalidad relacionada con los Bundles, seguridad, cache, y más. El Router es también un componente independiente llamado Routing.

## Componentes Symfony 2 versus Symfony Framework

Por ahora, hemos visto la mayoría de componentes básicos que componen el framework Symfony 2. En realidad todo sobre lo que hemos hablado hasta ahora ( la petición, la respuesta, el núcleo y el Router ) forman parte de tres componentes independientes utilizados en Symfony. De hecho cada característica de Symfony 2 pertenece a una de las más de veinte librerías independientes ( llamadas "Symfony Components" ). Incluso si has decidido construir tu propio framework PHP ( una idea poco inteligente ), puedes usar Symfony Components como partes de algunas capas funcionales. Y si tú usas Symfony 2 pero necesitas reemplazar por completo un componente y tienes la posibilidad de hacerlo. Symfony 2 esta desacoplado y se basa en un motor de inyección de dependencias orientado a interfaces. En otras palabras el desarrollador tiene un control completo.

¿Entonces, que es el Framework Symfony 2? El framework Symfony 2 es un PHP framework que cumple dos tareas distintas:

1. Facilitar una selección de componentes ( es decir Symfony 2 Componentes ) y librerías de terceros.
2. Facilitar una configuración que una muy bien todo.

El objetivo del framework es integrar algunas herramientas independientes para facilitar una experiencia completa al desarrollador. Incluso el propio framework es un bundle Symfony 2 que puede ser configurado o reemplazado por completo

Básicamente, Symfony 2 facilita un potente conjunto de herramientas para desarrollo rápido de aplicaciones web sin imponer nada en tu aplicación. Los usuarios normalmente pueden comenzar rápidamente sus desarrollos usando una distribución Symfony 2, que facilita un esqueleto al proyecto con parámetros por defecto. Para usuarios más avanzados el cielo es el límite.

Tradución libre de la entrada [The HTTP Spec and Symfony2](http://symfony.com/doc/2.0/book/http_fundamentals.html) Fundamentals del libro original de symfony 2.0
