---
title: "Manejo de excepciones en PHP"
date: 2011-02-28
tags: [php, oop]
source: https://desarrolla2.com/post/manejo-de-excepciones-en-php
---

# [Manejo de excepciones en PHP](https://desarrolla2.com/post/manejo-de-excepciones-en-php)

## ¿Que es una excepción?

Una excepción es un evento que ocurre durante la ejecución de un programa y requiere de la ejecución controlada de un bloque de código fuera del flujo de normal de ejecución.

El manejo de excepciones es una herramienta muy potente a la hora de realizar una gestión de una situacion. Lo primero que hay que comprender es que una excepción no es un error. Es una situación que se experimenta un bloque de código y que este no es capaz de manejar.

### ¿Por que debería usarlas?

Un correcto manejo de excepciones, pueden hacer su código mucho más fiable ahorrando dolores de cabeza.

### ¿Cuando lanzo una excepción y cuando un error?

Depende un poco de la política del proyecto, en el que estés trabajando, y los gustos de cada programador. Como con los nombres de las variables, lo mejor es llegar a una especie de estandar dentro de cada equipo. En general si estamos trabajando en una clase de alto nivel de una aplicación y muy acoplada con la misma, la mejor opción puede ser informar del error, mediante la gestión de errores de la propia aplicación, y tratar de controlarlo en este mismo lugar. Si por el contrario estamos trabajando en una clase de bajo nivel, desacoplada de la aplicación en la que estás trabajando y susceptible de ser reutilizada, lo mejor es lanzar una excepción y dejar que las clases de arriba traten de capturarlo, para informar al usuario.

### ¿Como lanzo una excepción?

Una excepción se lanza a través de la palabra reservada **thrown**, veamos un ejemplo.

```
<?php
function foo(){

    throw new Exception (' Upps !!');
}
foo();
// PHP Fatal error:  Uncaught exception 'Exception' with message ' Upps !!'
```

### ¿Entonces?

Cuando una excepción es lanzada PHP tratará de encontrar un bloque **catch{}** capaz de capturarla, si no encuentra ninguno, entonces, se interrumpirá la ejecución y se emitirá un error fatal de PHP con el mensaje "Uncaught Exception ...". Para "capturar" una excepción el código debe encontrarse dentro de un bloque **try{}** y debe tener al menos un bloque **catch{}**. Veamos un ejemplo.

```
<?php
function foo() {
    try {
        throw new Exception(' Upps !!');
    } catch (Exception $e) {
        // ..
    }
}

foo();
```

En este segundo ejemplo la ejecución del programa no se interrumpe a pesar de que se a lanzado una excepción. Veamos otro ejemplo más con varios bloques **catch**

```
<?php
function foo() {
    try {
        throw new Exception(' Upps !!');
    } catch (ErrorException $e) {
        // este bloque no se ejecuta, no coincide el tipo de excepción
        echo 'ErrorException' . $e->getMessage();
    } catch (Exception $e) {
        // este bloque captura la excepción
        echo 'Exception' . $e->getMessage();
    }
}

foo();
```

### Metodos de una excepción.

Una excepción es un objeto al fin y al cabo, a continuación vemos los métodos que tiene disponibles:

- getMessage() devuelve el mensaje de la expeción.
- getCode() devuelve el código del error.
- getFile() devuelve el nombre del fichero que lanzó la excepción.
- getLine() devuelve el la línea del fichero que lanzó la excepción.
- getTrace() devuelve la pila de ejeción en formato de array.
- getTraceAsString() devuelve la pila de ejeción en formato de string.

Puedes encontrar la referencia completa en el sitio oficial de php.

## Gestión de excepciones

Gracias a **set_exception_handler()** podemos controlar las excepciones no capturadas por bloques try/cacth. Gracias a esta caracteristica de PHP, podemos configurar que hacer cuando ocurre una excepción, como por ejemplo:

- Generar un log.
- Mostrar un mensaje de error de forma controlada.
- Enviar información completa a los administradores del sistema, para que puedan depurarlo.

Veamos un ejemplo para set_exception_handler:

```
<?php
/*
 * recupera información de la
 * maquina en la que se está efecutando
 *
 * @return string $info
 */

function getInfoMachine() { /* ... */
}

/*
 * recupera información de la
 * variables, $_POST, $_GET, $_SESSION
 *
 * @return string $info
 */

function getInfoEnviroment() { /* ... */
}

/*
 * recupera información del usuario que
 * esta ejecutando el proceso, en una aplicación
 * con gestión de usuarios.
 *
 * @return string $info
 */

function getInfoUser() { /* ... */
}

/*
 * recupera información de la excepción
 *
 * @param Exception $e
 * @return string $info
 */

function getInfoException(Exception $e) { /* ... */
}

/*
 * envia informacion a los administradores del sistema
 *
 * @param Exception $e
 * @return true or false
 */

function exception_handle(Excepion $e) {
    $message = getInfoMachine() . getInfoEnviroment() .
            getInfoUser() . getInfoException($e);
    mail($admins, $e->getMessage(), $message);
}

set_exception_handler('exception_handler');
```

## Creando tus propias excepciones

¿Por que querriamos crear un nuevo tipo de expceción? Muy fácil, para contemplar situaciones concretas, si por ejemplo estoy haciendo una librería que realiza tratamiento de imágenes, puedo querer añadir información extra a la excepción, relativa al tratamiento de imágenes.

```
<?php
class MyException extends Exception {/* ... */}
```

## Casos expeciales

A continuación veremos algunos casos expeciales que debemos conocer.

### Errores típicos

Un error típico cuando se trabaja con excepciones, y no se tiene experiencia, es omitir bloques de código que con necesarios al capturar excepciones, veamos un ejemplo más.

```
<?php
function foo() {
    DB::query('LOCK TABLES `a`, `b` WRITE');
    try {
        /* operaciones SQL */
    } catch (Exception $e) {
        /* operaciones SQL */
    }
    DB::query('UNLOCK TABLES');
}
```

En este ejemplo vemos como el código que desbloquea las tablas, se encuentra fuera del bloque catch, por lo que si ocurre una excepción las tablas se quedarán bloquedas. Por lo tanto el código catch, debe ocuparse de liberar las tablas.

```
<?php
function foo() {
    DB::query('LOCK TABLES `a`, `b` WRITE');
    try {
        /* operaciones SQL */
    } catch (Exception $e) {
        /* operaciones SQL */
  DB::query('UNLOCK TABLES');
    }
    DB::query('UNLOCK TABLES');
}
```

### Relanzar una excepción.

En algunas situaciones puedo querer relanzar una excepción, si por ejemplo quiero ejecutar un bloque de código al producirse, pero delegar en clases superiores el control final de la misma. Utilizaremos el mismo ejemplo del bloque anterior.

```
<?php
function foo() {
    DB::query('LOCK TABLES `a`, `b` WRITE');
    try {
        /* operaciones SQL */
    } catch (Exception $e) {
        /* operaciones SQL */
  DB::query('UNLOCK TABLES');
        throw $e;
    }
    DB::query('UNLOCK TABLES');
}
```

Nos vemos!
