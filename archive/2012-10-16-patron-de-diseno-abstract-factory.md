---
title: "Patron de diseño Abstract Factory"
date: 2012-10-16
tags: [php, oop]
source: https://desarrolla2.com/post/patron-de-diseno-abstract-factory
---

# [Patron de diseño Abstract Factory](https://desarrolla2.com/post/patron-de-diseno-abstract-factory)

## Abstract Factory

El objetivo es facilitar una interfaz para crear "familias" de objetos relacionados o dependientes sin que sea necesario especificar su clase.

Veamos mejor un ejemplo, supongamos que quiero definir una clase que genere coches.

```
`<?php  
  
abstract class AbstractCarFactory {
    abstract function makeCar();
    abstract function makeVan();
}`
```

La fabricación de coches tiene sus particularidades, por ejemplo en Europa los coches suelen ser más grandes, de mayor calidad, y más caros que los coches Asiaticos.

```
`<?php  
  
class EuropeanCarFactory extends ```AbstractCarFactory`{
    abstract function ```makeCar`(){  
        // ..  
````}
    abstract function ```makeVan`()```{  
        // ..  
````}`
}  
  
`  
`class AsianCarFactory extends ```AbstractCarFactory`{
    abstract function ```makeCar`()```{  
        // ..  
````};`
    abstract function ```makeVan`()```{  
        // ..  
````};`
}`
```

Ahora voy a definir cuales son las clases abstractas de los productos generados por estas factorias.

```
`<?php  
  
abstract class AbstractCar {
    abstract function getModelName();  
```    abstract function getPrice();`````````
}  
  
`  
`abstract class AbstractVan {
    abstract function getModelName();  
````    abstract function getPrice();``````````
}`
```

Ahora tendré que definir cuales serán las clases Car y Van para Europa y para Asia.

 

```
`<?php  
  
class EuropeanCar extends ```AbstractCar`{
    function getModelName()````{  
        return 'Renault Megane';  
````};  
``````    function getPrice()````{  
        return 10000;  
````};``
}  
  
  
`class EuropeanVan extends ```AbstractVan`{
    function getModelName()````{  
        return 'Renault Kangoo';  
````};  
``  
```````    function getPrice()````{  
        return 12000;  
````};``
}  
  
  
`  
class AsianCar extends ```AbstractCar`{
    function getModelName()````{  
        return 'Renault Megane';  
````};  
``````    function getPrice()````{  
        return 10000;  
````};``
}  
  
  
`class ```Asian`Van extends ```AbstractVan`{
    function getModelName()````{  
        return 'Renault Kangoo';  
````};  
``  
```````    function getPrice()````{  
        return 12000;  
````};``
}``````````````````````````
```
