---
title: "Imagen del día de la nasa como fondo de escritorio en gnome"
date: 2018-11-05
tags: [ubuntu, gnome, wallpaper]
source: https://desarrolla2.com/post/imagen-del-dia-de-la-nasa-como-fondo-de-escritorio-en-gnome
---

Una pequeña manía que tengo es tener fondos de pantalla que cambian cada cierto tiempo.

Exísten algunos paquetes que hacen esto para gnome, pero no están en los repositorios oficiales, e instalar un repositorio adicional, para algo tan sencillo, no acaba de convencerme.

Por eso tengo el un pequeño script en php que descarga una imagen y la pone como fondo de escritorio. En el ejemplo utilizo la [imagen del día de la NASA](https://www.nasa.gov/multimedia/imagegallery/iotd.html). Un servicio en el que cada día cuelgan una imagen nueva. La mayoría de las cuales bastante espectacular.

El script podría ser mucho más elaborado, pero prefería algo sencillo, sin dependencias.

```
`
<?php // run.php

$url = 'https://www.nasa.gov/rss/dyn/lg_image_of_the_day.rss';
$directory = '/your/path/to/download/directory';
if (!is_dir($directory)) {
    exec(sprintf('mkdir -p %s', $directory));
}
$feed = file_get_contents($url);
$xml = simplexml_load_string($feed);
$json = json_encode($xml);
$array = json_decode($json, true);

if (!array_key_exists('channel', $array)) {
    return;
}
$channel = $array['channel'];

if (!array_key_exists('item', $channel)) {
    return;
}

$items = $channel['item'];
foreach ($items as $item) {
    if (!array_key_exists('enclosure', $item)) {
        continue;
    }

    $enclosure = $item['enclosure'];
    if (!array_key_exists('@attributes', $enclosure)) {
        continue;
    }

    $attributes = $enclosure['@attributes'];

    if (!array_key_exists('url', $attributes)) {
        continue;
    }

    $url = $attributes['url'];
    $path_info = pathinfo($url);
    $extension = $path_info['extension'];
    $fileName = sprintf('%s/%s.%s', $directory, md5($url), $extension);
    if (file_exists($fileName)) {
        return;
    }

    exec(sprintf('wget -O %s %s', $fileName, $url));
    exec(sprintf('gsettings set org.gnome.desktop.background picture-uri file://%s', $fileName));

    return;
}`
```

Hay otros servicios que te ofrecen buenas imágenes en formato rss como por ejemplo [unspash](https://unsplash.com/search/photos/rss). Seguro que puedes encontrar un servicio que te de un tipo de imágenes de tu agrado.

Cada vez que ejecutes este fichero, buscar actualizaciones, descarga la última y la configura como fondo de escritorio.

```
`php run.php`
```

Yo programo una ejecución periódica del mismo, añadiendo el siguiente contenido en el fichero /etc/cron.d/wallpaper

```
`10 * * * *   yourusername /usr/bin/php /path/to/script/run.php >> /tmp/cron.nasa-wallpaper.log 2>&1`
```

Ya me contarás si te ha sido útil.
