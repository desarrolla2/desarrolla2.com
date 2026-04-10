---
title: "El patrón Observer"
date: 2012-12-17
tags: [php, oop, patrones]
source: https://desarrolla2.com/post/el-patron-observer
---

# [El patrón Observer](https://desarrolla2.com/post/el-patron-observer)

El patrón "Observer" también conocido como el patrón "Publish-Subscribe" es un patrón de diseño de[ comportamiento](http://es.wikipedia.org/wiki/Patr%C3%B3n_de_dise%C3%B1o#Patrones_de_comportamiento), que define una relación de uno a muchos de tal manera que cuando un objeto cambia su estado, todos los objetos dependientes son notificados de la actualización automáticamente.

Este patrón como veremos, resuelve el problema del "crecimiento" de nuestras clases, según nuestra aplicación va ganando funcionalidad.

El objeto con una relación de uno a muchos que están suscritos a sus actualizaciones de estado se denomina el "subject" o el "publisher". Los objetos dependientes de el son llamados "observers" o "sucriptors".

Los objetos "observers" son notificados de cada actualización del "subject" y actúan en consecuencia. Un objeto "subject" puede tener cualquier número de "observers".  
Si te das cuenta es una forma sencilla de aumentar la funcionalidad de una clase, sin modificar la misma.

## La clase Observer

En el ejemplo la clase "observer" facilita un método update() que será llamado por el "subject" para notificar una actualización de su estado. En el ejemplo he definido el método update() pero si estás utilizando este patrón seguramente quieras definirlo como abstracto y delegar en las subclases de "observer" la implementación concreta.

```
<?php 
abstract class Observer
{
    public function update(Subject $subject) {
    // observer's stuff 
    }
}
```

## La clase Subject

La clase "Subject" es también una clase abstracta que define los siguientes métodos: *attach*(), *detach*() y *notify()*, que son los responsables de añadir y eliminar "Observers" y de notificar que ha existido una actualización de estado. Tambien he añadido otro método *setState*(), que será el responsable de desencadenar el *notify*().

```
<?php abstract class Subject
{
    protected $observers;
    protected $state;

    public function __construct() {
        $this->observers = array();
        $this->state = null;
    }

    public function attach(Observer $observer) {
        $i = array_search($observer, $this->observers);
        if ($i === false) {
            $this->observers[] = $observer;
        }
    }

    public function detach(Observer $observer) {
        if (!empty($this->observers)) {
            $i = array_search($observer, $this->observers);
            if ($i !== false) {
                unset($this->observers[$i]);
            }
        }
    }

    public function setState($state) {
        $this->state = $state;
        $this->notify();
    }

    public function notify() {
        if (!empty($this->observers)) {
            foreach ($this->observers as $observer) {
                $observer->update($this);
            }
        }
    }
}
```

El método *attach*() añade un suscribe un "observer" al "subject" así que cualquier cambio de estado será notificado al suscriptor y este puede actuar en consecuencia. El metodo *detach*() se encarga de eliminar esa suscripcion.

Finalmente el metodo *notify*() se encarga de notificar a cada uno de los suscriptores.

## Resumen

El patron Observer permite diferenciar entre la lógica principial de una clase y la lógica opcional. Este patrón es muy util a la hora de mantener nuestra logica de aplicación desacoplada, modular y sobre todo extensible a los nuevos requerimientos que pueda tener nuestra aplicación.

Además es muy util a la hora de permitir una reimplementación sencilla de cualquiera de los "suscribers" ya que como se ve, la implementación de cada uno de ellos es totalmente independiente de los demás y del propio "subject"
