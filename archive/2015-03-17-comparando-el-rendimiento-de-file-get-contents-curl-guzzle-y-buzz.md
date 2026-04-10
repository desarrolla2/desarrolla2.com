---
title: "Comparando el rendimiento de file_get_contents, curl, guzzle y buzz"
date: 2015-03-17
tags: [php, performance]
source: https://desarrolla2.com/post/comparando-el-rendimiento-de-file-get-contents-curl-guzzle-y-buzz
---

Para un proyecto que vamos a comenzar dentro de la compañía, y para el cual el rendimiento es muy
  importante, hemos realizado una comparación entre diferentes clientes que pueden realizar una petición
  http.

Nuestra idea original era usar guzzle, pero como digo el rendimiento es importante, por lo que antes de usarlo,
  decidimos realizar algunos test de rendimiento.

Los requisitos que tiene que cumplir nuestro cliente son:

- Poder comprobar códigos de estado en las respuestas.
- Gestión avanzada de cabeceras ( petición y respuesta ).
- Poder configurar timeouts.
- En la medida de lo posible, que sea rápido y consuma poca memoria.

Las pruebas se realizadan contra un servidor local, el código para las pruebas es este.

```
`<?php
/*
 * This file is part of the XXX package.
 *
 * (c) Daniel González <daniel@desarrolla2.com>
  *
 * For the full copyright and license information, please view the LICENSE
 * file that was distributed with this source code.
 */

require __DIR__.'/vendor/autoload.php';

use Desarrolla2\Timer\Timer;
use Desarrolla2\Timer\Formatter\Human;

$url = 'http://localhost/';
$times = 1000;
$repeat = 5;
$timer = new Timer();
for ($r = 1; $r <= $repeat; $r++) {
    for ($i = 1; $i <= $times; $i++) {
        $currentUrl = $url.'?id='.$r.$i;
        file_get_contents($url);
    }
    $mark = $timer->mark('file_get_contents');
    for ($i = 1; $i <= $times; $i++) {
        $currentUrl = $url.'?id='.$r.$i;
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($ch, CURLOPT_URL, $url);
        $output = curl_exec($ch);
        $code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    }
    $mark = $timer->mark('curl_exec');
    for ($i = 1; $i <= $times; $i++) {
        $currentUrl = $url.'?id='.$r.$i;
        $client = new GuzzleHttp\Client();
        $response = $client->get($currentUrl);
        $code = $response->getStatusCode();
    }
    $mark = $timer->mark('Guzzle');
    for ($i = 1; $i <= $times; $i++) {
        $currentUrl = $url.'?id='.$r.$i;
        $browser = new Buzz\Browser();
        $response = $browser->get($currentUrl);
    }
    $mark = $timer->mark('Buzz');
}
$marks = $timer->getAll();
$results = [];
foreach ($marks as $mark) {
    if (!isset($results[$mark['text']])) {
        $results[$mark['text']] = [
            'time' => 0,
            'memory' => 0
        ];
    }
    $results[$mark['text']]['time'] += $mark['time']['from_previous'];
    $results[$mark['text']]['memory'] += $mark['memory']['from_previous'];
}

$formmater = new Human();
foreach ($results as $key => $value) {
    $results[$key]['time'] = $formmater->time($value['time']);
    $results[$key]['time_by_request'] = $formmater->time($value['time']/$repeat/ $times);
    $results[$key]['memory'] = $formmater->memory($value['memory']);
}

var_dump($results);
`
```

El script realiza 10,000 peticiones con cada uno de los métodos, sucesivamente, repitiendo la operación
  5 veces, es decir 10,000 * 5 * 5 = 250,000 peticiones en total.

En cada request inicializamos el cliente y realizamos una petición, repitiend la operación 10,000 veces
  con cada método. La petición se realiza a un servidor en local que regresa un documento pequeño
  en torno a 12K, de esta forma podemos ver exactamente cual es el performance del cliente.

## Ejemplo 1: file_get_contents

file_get_contents, es la forma más sencilla de ejecutar una petición http, aunque no cumple los
  requisitos ya que necesitamos poder comprobar códigos de estado, y añadir timeouts por lo que este método
  no era válido. Aún así lo añadimos, por simple curiosidad.

tiempo por 10,000

1.52s

tiempo medio por request

0.3ms

consumo de memoria

0

### Conclusiones:

No incrementa el consumo de memoria, y funciona muy rápido, por debajo del milisegundo por
  petición.

## Ejemplo 2: curl

Es una forma muy simple de realizar peticiones HTTP, pero además nos permite hacer una gestión avanzada
  de cabeceras y códigos de estado, lo cual era un requisito indispensable para nosotros.

tiempo por 10,000

1.3s

tiempo medio por request

0.26ms

consumo de memoria

0

### Conclusiones:

Funciona aún más rápido que el ejemplo anterior, tampoco añade consumo de memoria, y además
  cumple los requisitos, por lo que parece un gran candidato.

## Ejemplo 3, guzzle

Vamos a usar la última versión estable la 5.1. Usar Guzzle era nuestra idea original, cumple todos los
  requisitos y además ofrece una API con la que estamos muy familiarizados para trabajar con peticiones HTTP.

tiempo por 10,000

3.29s

tiempo medio por request

0.66ms

consumo de memoria

1.25MB

### Conclusiones:

Es un poco más lento pero aún así sigue el tiempo de ejecución de cada petición
  está por debajo del milisegundo, por lo que también parece un buen candidato, lo peor en comparación
  con curl es que si añade consumo de memoria. En el entorno en el que tenemos previsto ese 1,25M por request
  puede ser un gran consumo de memoria.

## Ejemplo 4, buzz

Hemos querido añadir también este popular cliente para ver que tal se comporta. Aunque en realidad
  parece que está un poco abandonado, además no permite de manera sencilla recuperar los códigos de
  estado http, por lo que tampoco cumple los requisitos.

tiempo por 10,000

1.74s

tiempo medio por request

0.35ms

consumo de memoria

256KB

### Conclusiones:

Es rápido y apenas aumenta el consumo de memoria.

## Conclusiones finales

Para nuestro caso concreto tendremos que decidir, entre usar curl o guzzle ya que son los que cumplen nuestros
  requisitos, quizá ese 1.25M de más en memoria que ocupa guzzle sea lo que finalmente mueva la balanza de
  un lado a otro.

En una plataforma donde el performance no sea crítico creemos que no merece la pena usar curl y si usar guzzle
  por la elegante API que facilita y la poca diferencia en el rendiemiento entre ambas. DRY.
