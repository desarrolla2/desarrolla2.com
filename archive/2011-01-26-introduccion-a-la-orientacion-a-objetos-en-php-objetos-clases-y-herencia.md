---
title: "Introducción a la orientación a objetos en php objetos, clases y herencia"
date: 2011-01-26
tags: [php, oop]
source: https://desarrolla2.com/post/introduccion-a-la-orientacion-a-objetos-en-php-objetos-clases-y-herencia
---

# [Introducción a la orientación a objetos en php objetos, clases y herencia](https://desarrolla2.com/post/introduccion-a-la-orientacion-a-objetos-en-php-objetos-clases-y-herencia)

Este es el primer capítulo de una serie que pretende cubrir los aspectos de la orientación a objetos en PHP5. **¿Para que sirve la programación orientada a objetos?** La programación orientada a objetos en adelante OOP por sus siglas en inglés.

Es un paradigma que sirve para generar código, más seguro, modular, y más fácil de mantener a lo largo del tiempo. Despues de leer este y los siguientes capitulos de la serie deberías comprender al menos los siguientes conceptos:

- Clases
- Objetos
- Metodos
- Propiedades
- Instanciación
- Constructores
- Métodos mágicos
- Visibilidad
- y otros muchos ...

La OOP desarrolla nuevos conceptos y terminologías que deben comprenderse correctamente para generar buen código.

## ¿Qué es una clase?

Una clase, es la definición de una entidad que tiene características y métodos relacionados entre sí, en cualquier libro de programación encontrarás un capítulo dedicado a la orientación a objetos, no es el objetivo de este artículo explicar en profundidad el paradigma, si no realizar una aproximación que permita comprender los conceptos.

Así por ejemplo la clase Vehiculo podría ser la siguiente:

```
`<?php
    class Vehiculo {  

        public $matricula = '';
        public $velocidad = 0;

        public function aumentarVelocidad() {
            $this->velocidad++;
        }

        public function disminuirVelocidad() {
            $this->velocidad--;
        }

        public function parar() {
            $this->velocidad = 0;
        }

    }`
```

Mediante la palabra reservada **class** definimos una clase completa.

Vemos las declaraciones de las variables $matricula y $velocidad, igual que en la programación prodecimental, las variables son contenedores de datos, solo que ahora hablaremos de **propiedades**.

A continuación veremos que mediante la palabra reservada **function** la definición de las funciones *aumentarVelocidad*(), *disminuirVelocidad*() y *parar*().

Cuando estamos trabajando en un entorno de OPP en lugar de llamarlos **funciones** las llamamos **métodos**.

Una buena aproximación consiste en programar siguiendo esquemas de objetos reales, por lo que seguramente serás capaz de imaginar otros métodos que podría tener la clase Vehiculo, quizá: *frenar*(), *derrapar*(), y por supuesto *irALaParteDeAtras*().

Como ves puedes identificar las propiedades de una clase con las caracterisiticas, y sus métodos con las acciones que esta puede realizar.

## ¿Entoces que es un objeto?

Las clases son solo una definición, es decir una plantilla, tal y como vimos en el ejemplo la clase Vehiculo me permite describir las propiedades y métodos de los vehiculos en general, pero aún no puedo utilizarlos para un vehiculo en particular. Para ello es necesario instanciar una clase en un **objeto** tal y como vemos en el siguiente ejemplo:

```
`<?php
    $coche1 = new Vehiculo();
    $coche1->aumentarVelocidad();
    // velocidad = 1

    $coche2 = new Coche();`
```

Queda claro que el **coche1** tiene velocidad 1, mientras que el **coche2** aún está parado.

El proceso por el cual utilizo una clase para crear un objeto se denomina instanciación y se identifica mediante la palabra reservada **new**. Avancemos un poco más.

## Herencia

La herencia consiste la capacidad en reutilizar código a través de clases padre y clases hijo, que heredan los métodos y propiedades de este, así por ejemplo si quisiera definir una nueva clase que representara a un coche deportivo, podría ser la siguiente:

```
`<?php
    class Deportivo extends Coche {

        var $aleron_deportivo = true;
        var $cristales_tintados = true;
        var $musica_maquinera = 0;

        function ponerMusica() {
            $this->musica_maquinera = 10;
        }

        function apagarMusica() {
            $this->musica_maquinera = 0;
        }

    }

    $deportivo = new Deportivo();

    $deportivo->aumentarVelocidad();
    // velocidad = 1
    $deportivo->aumentarVelocidad();
    // velocidad = 2
    $deportivo->ponerMusica();
    // musica = 10`
```

Por lo tanto utilizamos la herencia para definir clases generales, y a través de clases hijos, ir definiendo clases más concretas, afinando su comportamiento, por ejemplo podríamos crear una clase Ferrari, que heredara a su vez de Deportivo, para añadirle características más específicas, o crear una nueva clase que herede de Coche, llamada furgoneta, que aumente su velocidad más despacio, pero con mayor capacidad de carga. La herencia es el mecanismo fundamental de la OOP,
