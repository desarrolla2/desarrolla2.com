---
title: "Sobre escribiendo los métodos getter y setter en doctrine"
date: 2011-05-26
tags: [php, symfony1, oop]
source: https://desarrolla2.com/post/sobre-escribiendo-los-metodos-getter-y-setter-en-doctrine
---

# [Sobre escribiendo los métodos getter y setter en doctrine](https://desarrolla2.com/post/sobre-escribiendo-los-metodos-getter-y-setter-en-doctrine)

Cuando tratamos de sobreescribir un metodo getter de doctrine, y queremos acceder al valor del campo, lo normal es que llamemos al metodo del padre, veamos cómo:

```
    public function getName(){
        if ($name = parent::getName()){
            return $name;
        }else{
            return '---';
        }
    }

```

Sin embargo esto nos lanzará un error del tipo **"Fatal error: Maximum function nesting level of '100' reached, aborting!"**.

Este comportamiento se debe a que en Doctrine cuando utilizamos un método "getter" se invoca el método mágico "__call()", que a su vez chequea si existe un acceso personalizado para el campo, que es lo que justo acabamos de crear con nuestro método getName() del ejemplo.

Veamos en el siguiente ejemplo cómo se soluciona

```
    public function getName() {
        if ($name = parent::_get('name')) {
            return $name;
        } else {
            return '---';
        }
    }

```
