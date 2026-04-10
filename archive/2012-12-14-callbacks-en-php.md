---
title: "Callbacks en PHP"
date: 2012-12-14
tags: [php]
source: https://desarrolla2.com/post/callbacks-en-php
---

# [Callbacks en PHP](https://desarrolla2.com/post/callbacks-en-php)

Los [callbacks](http://en.wikipedia.org/wiki/Callback_(computer_programming)) es un tipo de funciones que son pasadas como parametros y que serán ejecutadas desde una subrutina. Se suelen utilizar para ejecutar algo, una vez que la rutina llamada a terminado su ejecución, y además se suelen utilizar [funciones anónimas](http://en.wikipedia.org/wiki/Anonymous_function) para ser pasadas como parametros.

Los callbacks es algo muy poco utilizado en PHP, no como en otros lenguajes como javascript, esto no quiere decir que no existan a continuación un ejemplo.

```
`<?php

function myFunctionWithCallback($data, callable $callback)
{
    // process $data 
    call_user_func($callback, $data);
}

myFunctionWithCallback(1, function($data) {
            var_dump($data);
        });`
```

Como podeís ver hemos tipado el segundo parametro, para asegurarnos que nos pasan algo que después vamos a poder llamar.

Aqui teneís la referencia completa del [tipo callable](http://php.net/manual/en/language.types.callable.php).

Seguro que puede seros interesante para alguno de vuestros proyectos.
