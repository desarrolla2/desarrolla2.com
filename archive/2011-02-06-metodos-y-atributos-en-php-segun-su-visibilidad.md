---
title: "Métodos y atributos en PHP según su visibilidad"
date: 2011-02-06
tags: [php, oop]
source: https://desarrolla2.com/post/metodos-y-atributos-en-php-segun-su-visibilidad
---

# [Métodos y atributos en PHP según su visibilidad](https://desarrolla2.com/post/metodos-y-atributos-en-php-segun-su-visibilidad)

En este segundo artículo de la serie "orientación a objetos en PHP5", vamos a profundizar en los diferentes tipos de **clases**, **métodos** y **atributos** disponibles en **PHP5**, y cuales son sus características.

## Visibilidad de las variables

### Atributos de tipo public

Las variables en PHP5 son declaradas **public** a menos que se indique lo contrario, esto significa que se puede interactuar con ellas desde el exterior.

```
`<?php
class Coche {
    $velocidad = 0;

    function aumentarVelocidad() {
        $this->velocidad++;
    }

    function parar() {
        $this->velocidad = 0;
    }
}
$coche = new Coche();
$coche->velocidad = 1000; // velocidad = 1000`
```

### Atributos de tipo private

Esta forma de actuar rompe con la filosofía de la **programación orientada a objetos**, donde como norma general las propiedades deben estar ocultas al entorno exterior, para ello podemos usar el modificador, **private**.

```
`<?php
class Coche {
    private $velocidad = 0;

    function aumentarVelocidad() {
        $this->velocidad++;
    }

    function parar() {
        $this->velocidad = 0;
    }

}
$coche = new Coche();
$coche->velocidad = 1000; // PHP Fatal error`
```

En este ejemplo nuestra propiedad velocidad, se encuentra más protegida a ser modificada o accedida desde contextos que no queremos que tengan acceso a ella. Es común permitir el acceso a las propiedades de forma controlada a través de "**setters**" y "**getters**", veamos en un ejemplo a lo que me refiero:

```
`<?php
class Coche {
    private $velocidad = 0;

    function setVelocidad($velocidad) {
        // Aquí se podrían implementar otras condiciones como $velocidad > 0, etc ..
        $this->velocidad = (int) $velocidad;
    }

    function getVelocidad() {
        return $this->velocidad;
    }

}
$coche = new Coche();
$coche->setVelocidad(1000);
$coche->getVelocidad(); // 1000`
```

### Atributos de tipo protected

¿Que ocurre con las clases hijas? ¿puede utilizar una propiedad de tipo **private**? Veamos que ocurre con nuestro deportivo:

```
`<?php
class Coche {
    private $velocidad = 0;

    function setVelocidad($velocidad) {
        $this->velocidad = (int) $velocidad;
    }
}

Class Deportivo extends Coche{

    function aumentarVelocidad() {
        $this->velocidad  = $this->velocidad * 2;
    }
}

$deportivo = new Deportivo();
$deportivo->setVelocidad(100);
$deportivo->aumentarVelocidad(); // PHP Notice:  Undefined property Deportivo::$velocidad`
```

Los métodos que heredan de Coche, pueden acceder a la propiedad declarada como **private**, sin embargo cualquier método nuevo no puede. Para ello existe un nuevo tipo de ámbito, **protected**, que permite el acceso completo desde la propia clases y todas las clases heredadas.

```
`<?php
class Coche {
    protected $velocidad = 0;

    function setVelocidad($velocidad) {
        $this->velocidad = (int) $velocidad;
    }
}

Class Deportivo extends Coche{

    function aumentarVelocidad() {
        $this->velocidad  = $this->velocidad * 2;
    }
}

$deportivo = new Deportivo();
$deportivo->setVelocidad(100);
$deportivo->aumentarVelocidad(); // velocidad = 200`
```

Una buena práctica es declarar siempre los atributos como **private** o **protected**, si la clase es susceptible de ser heredado y quieras delegar el control del atributo a las clases hijas. después, utilizar métodos públicos para acceder o modificar su valor. ¿Y como son estos comportamientos en los métodos?

### Métodos de tipo public, private y protected

Pues la lógica es la misma, todos los métodos son declarados como public a menos que se indique lo contrario, todos los métodos de los ejemplos vistos hasta ahora son de tipo **public** y por esto han podido ser accedidos desde el exterior. Veamos ahora un par de ejemplos más con métodos **private** y **protected**.

```
`<?php
class Coche {
    protected $velocidad = 0;
    private $combustible = 10;

    function setVelocidad($velocidad) {
        if ($this->getCombustible()){
            $this->velocidad = (int) $velocidad;
        }else{
            $this->velocidad = 0;
        }
    }

    private function getCombustible(){
        return $this->combustible;
    }
}

Class Deportivo extends Coche{

    function aumentarVelocidad() {
        if ($this->getCombustible()){
            $this->velocidad  = $this->velocidad * 2;
        }else{
            $this->velocidad = 0;
        }
    }
}

$deportivo = new Deportivo();
$deportivo->setVelocidad(100);
$deportivo->aumentarVelocidad();  // PHP Fatal error:  Call to private method Coche::getCombustible()`
```

Al ser *getCombustible*() declarado como privado, puede ser accedida desde los métodos del padre, pero no desde los métodos del hijo, en este caso al ser una propiedad a la que necesitamos acceder desde la clase hijo, debemos declararla como **protected**. En el próximo artículo veremos los modificadores **final**, **static** y **abstract**, y su efecto en clases, **métodos** y **atributos**.
