---
title: "Otros tipos de clases, métodos y atributos."
date: 2011-03-26
tags: [php, oop]
source: https://desarrolla2.com/post/otros-tipos-de-clases-metodos-y-atributos
---

# [Otros tipos de clases, métodos y atributos.](https://desarrolla2.com/post/otros-tipos-de-clases-metodos-y-atributos)

En este artículo profundizaremos en el conocimiento de la orientación a objetos en PHP5, con clases y metodos abstractos, estaticos y finales.

## Clases y metodos abstractos

Las clases abstractas son aquellas que no pueden ser instanciadas. Las clases que contengan al menos un método abstracto deberán ser definidas como abstractas. Una clase abstracta es parecida a una interfaz (las cuales veremos en otro capítulo) con dos diferencias principales.

Una clase solo puede heredar de otra, pues no existe herencia multiple en PHP5, sin embargo puede implementar tantas interfaces como queramos.

```
< ?php

abstract class Persona {
    // ..
}

interface Estudiante {
    // ..
}

interface Empleado {
    // ..
}

class Mario extends Persona  implements Estudiante, Empleado {
    // ..
}

```

Esta limitación en herencia respecto a otros lenguajes nos obliga a esforzarnos más a la hora de diseñar nuestras colecciones de objetos, y puede obligarnos a hacer alguna que otra chapuza.

La segunda diferencia con las interfaces es que mientras que estas solo pueden declarar métodos y propiedades, las clases abstractas pueden definirlos, veamos un nuevo ejemplo.

```
< ?php

abstract class Persona {
    
    private $cansado;

    public function dormir(){
        $this->cansado = false;
    }

    public function caminar(){
        $this->cansado = true;
    }
}

interface Estudiante {
    public function estudiar();
}

```

Por lo tanto nos decantaremos por usar una interfaz si tan solo queremos definir un comportamiento, delegando completamente la implementación, y utilizaremos una clase abstracta si queremos implemetar parte o todo ese comportamiento.

Si quisieramos simplemente definir un método pero obligar a que sea definido en las clases que heredan de esta tan solo tenemos que definirla tambien como abstracta.

```
< ?php

abstract class Persona {

    private $cansado;

    public function dormir() {
        $this->cansado = false;
    }

    public function caminar() {
        $this->cansado = true;
    }

    abstract public function comer();
}

class Mario extends Persona {

    public function comer() {
        $this->cansado = false;
    }

}

```

Los metodos declarados como abstractos, tienen que ser definidos, con un nivel de visibilidad igual o menos restrictivo, es decir

- private: private, protected o public
- protected: protected o public
- public: public

Finalmente las propiedades no pueden ser definidas como abstractas.

```
< ?php

abstract class Persona {

    abstract private $cansado;
    // PHP Fatal error:  Properties cannot be declared abstract
}

```

## Clases y métodos estáticos

Aquellos métodos definidos como estáticos, pueden ser accedidos sin la necesidad de instanciar la clase que los contiene.

No es necesaria ninguna palabra reservada para declarar un metodo estático, podemos acceder a cualquier método de forma estática con el operador ::, aunque mi recomendación es que indiquemos que se trata de un método diseñado para accederse como estatico utilizando la palabra reservada static en su declaración.

```
< ?php

abstract class Persona {

    private $cansado;

    public function dormir() {
        echo 'descansado';
    }

    public function caminar() {
        $this->cansado = true;
    }

}
Persona::dormir();
// descansado

```

Por supuesto el interprete lanzará un error si dentro de un método al que estamos accediendo de forma estática con el operador :: se accede a otro método o atributo que no lo es.

```
< ?php

abstract class Persona {

    private $cansado;

    public function dormir() {
        $this->cansado = false;
    }

    public function caminar() {
        $this->cansado = true;
    }

}

Persona::dormir();
// PHP Fatal error:  Using $this when not in object context 

```

Como hemos visto al no tratarse de un objeto instanciado de la clase, no tenemos disponible la referencia al objeto $this, en su lugar tenemos una referencia a la clase estatica self::

```
< ?php

abstract class Persona {

    private static $cansado;

    public function dormir() {
        self::$cansado = false;
    }


}

Persona::dormir();

```

Tradicionalmente se utilizaban clases con metodos estaticos como una especie de espacios de nombres agrupando en una misma clase funciones que tienen responsabilidades similares, asi por ejemplo en la clase Validator tengo disponibles métodos genericos, Validator::email(), Validator::integer(). Es una buena idea si te encuentras con esta casuistica.

## Clases y métodos Finales

Por último veremos las clases y métodos finales. Mediante la palabra reservada final impimos que se pueda heredar de una clase.

```
< ?php

final class Persona {

    public function dormir() {
        // ..
    }

}

class Mario extends Persona {
    // PHP Fatal error:  Class Mario may not inherit from final class (Persona)

    public function caminar() {
        // ..
    }

}

```

Tratandose de un método, lo que impide es la sobreescritura del mismo.

```
< ?php

class Persona {

    public final function dormir() {
        // ..
    }

}

class Mario extends Persona {

    public function dormir() {
        // PHP Fatal error:  Cannot override final method Persona::dormir()
    }

}

```

Gracias a este caracteristica del PHP podemos garantizar el comportamiento de las clases que heredan de las que estamos definiendo, o incluso garantizar que no se heredará de ella. Recordemos que las clases hijas
