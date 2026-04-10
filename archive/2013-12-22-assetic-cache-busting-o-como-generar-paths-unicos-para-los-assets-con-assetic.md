---
title: "Assetic Cache Busting o como generar paths únicos para los assets con assetic"
date: 2013-12-22
tags: [php, symfony, cache, assetic]
source: https://desarrolla2.com/post/assetic-cache-busting-o-como-generar-paths-unicos-para-los-assets-con-assetic
---

# [Assetic Cache Busting o como generar paths únicos para los assets con assetic](https://desarrolla2.com/post/assetic-cache-busting-o-como-generar-paths-unicos-para-los-assets-con-assetic)

Un problema que puede dar lugar el uso de assetic es invalidar la cache, es decir que los recursos que se generaron en un deploy y que en mi caso están cacheados tanto en el cliente ( http cache ) como en el servidor ( varnish ) y ahora es necesario que el navegador use las nuevas versiones.

La solución es sencilla cambiar los nombres de dichos recursos.

Hasta la última versión de symfony, es decir, hasta la 2.3 había que crear un AssetFactory tal y como indico aqui

```
<?php


namespace Desarrolla2\Bundle\WebBundle\Factory;

use Symfony\Bundle\AsseticBundle\Factory\AssetFactory as BaseAssetFactory;
use Assetic\Factory\LazyAssetManager;
use Assetic\Factory\Worker\CacheBustingWorker;
use Symfony\Component\DependencyInjection\ContainerInterface;
use Symfony\Component\DependencyInjection\ParameterBag\ParameterBagInterface;
use Symfony\Component\HttpKernel\KernelInterface;

/**
 * Class AssetFactory
 */
class AssetFactory extends BaseAssetFactory
{
    public function __construct(
        KernelInterface $kernel,
        ContainerInterface $container,
        ParameterBagInterface $parameterBag,
        $baseDir,
        $debug = false
    ) {
        parent::__construct($kernel, $container, $parameterBag, $baseDir, $debug);
        $this->addWorker(
            new CacheBustingWorker(new LazyAssetManager(new BaseAssetFactory(
                $kernel,
                $container,
                $parameterBag,
                $baseDir,
                $debug
            )))
        );
    }
}
```

Y luego publicar el worker

```
parameters:
  assetic.asset_factory.class: Desarrolla2\Bundle\WebBundle\Factory\AssetFactory

```

Ahora desde la versión 2.4 esto es mucho más sencillo, solo necesitas incluir esto en la configuración

```
assetic:
    workers:
        cache_busting: ~

```

Ambas soluciones la que funciona hasta la 2.3 y la que funciona en la 2.4, son muy sencillas de implementar, pero están escondidas en comentarios de PR, y no documentadas, por lo que es bastante dificil encontrarlas.
