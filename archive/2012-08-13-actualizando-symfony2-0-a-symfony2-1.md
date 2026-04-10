---
title: "Actualizando Symfony2.0 a Symfony2.1"
date: 2012-08-13
tags: [php, symfony]
source: https://desarrolla2.com/post/actualizando-symfony2-0-a-symfony2-1
---

# [Actualizando Symfony2.0 a Symfony2.1](https://desarrolla2.com/post/actualizando-symfony2-0-a-symfony2-1)

Antes de enfrentar la tarea de actualizar el sitio web de [symfony madrid](http://symfony-madrid.es) a la versión 2.1 decidí ir anotando todos los pasos, por si a alguien le podían llegar a ser últiles.

Lo primero fue obtener una copia de composer.json de la distribución estandar.

```
`wget https://raw.github.com/symfony/symfony-standard/master/composer.json`
```

y descargar el binario de composer, si es que no lo tienes ya instalado.

```
`curl -s https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer`
```

## Actualizando dependencias

Esta es la parte más engorrosa de todas, consiste en cambiar las antiguas dependencias del fichero deps al nuevo formato basado en composer. Symfony Madrid tenía algunas dependencias extra, por lo que aquí dedique un rato. En tu caso dependerá mucho de lo actualizadas que estén tus dependencias y del número que tengas.

### Doctrine Migrations

```
[doctrine-migrations]
    git=http://github.com/doctrine/migrations.git

[DoctrineMigrationsBundle]
    git=http://github.com/doctrine/DoctrineMigrationsBundle.git
    target=/bundles/Symfony/Bundle/DoctrineMigrationsBundle
```

No encontre la correspondencia de esta dependencia, es necesario comentar la referencia en el AppKernel.php, seguramente estaŕa por hay, pero tampoco era una dependencia necesaria así que no le dedique mas tiempo.

```
`$bundles = array(
            ...
            //new Symfony\Bundle\DoctrineFixturesBundle\DoctrineFixturesBundle(),`
```

### Doctrine Extensions

```
[doctrine-extensions]
    git=http://github.com/beberlei/DoctrineExtensions.git
```

Tampoco encontre esta dependencia, posiblemente se encuentre dentro de alguno de los repositorios de doctrine ahora.

### Doctrine Fixtures

```
[doctrine-fixtures]
    git=http://github.com/doctrine/data-fixtures.git
```

```
[DoctrineFixturesBundle]
    git=http://github.com/doctrine/DoctrineFixturesBundle.git
    target=/bundles/Symfony/Bundle/DoctrineFixturesBundle
```

Añadi la dependencia.

```
`"require": {
    ...
    "doctrine/doctrine-fixtures-bundle": "dev-master" 
}`
```

### LadyBug

```
[Ladybug]
    git=http://github.com/raulfraile/Ladybug.git
    target=ladybug

[RaulFraileLadybugBundle]
    git=http://github.com/raulfraile/LadybugBundle.git
    target=bundles/RaulFraile/Bundle/LadybugBundle
            
```

Añadi la siguiente dependencia.

```
`"require": {
    ...
    "raulfraile/ladybug-bundle": "dev-master" 
}
`
```

### TinymceBundle

```
[TinymceBundle]
    git=git://github.com/stfalcon/TinymceBundle.git
    target=/bundles/Stfalcon/Bundle/TinymceBundle
```

Me gusta mantener al mínimo la cantidad de Bundles de terceros, y en concreto este no es necesario. Configurar TinyMCE es sencillo sin necesidad de tener que utilizar un bundle externo. Elimino la referencia en el AppKernel.php.

### FeedBundle

```
[FeedBundle]
    git=http://github.com/Nek-/FeedBundle.git
    target=/bundles/Nekland/FeedBundle
```

Este bundle en concreto generaba un problema desde tiempos remotos en symfony-madrid, que en alguna ocasión nos ha dejado en mal lugar, por generar un error 500 en todo el sitio, no se exactamente cual será el problema, pero creo que este bundle tampoco aporta lo suficiente como para ponerse a investigar. Elimino la referencia en el AppKernel.php.

### RSSClientBundle

```
[RSSClientBundle]
    git=http://github.com/desarrolla2/RSSClientBundle.git
    target=/bundles/Desarrolla2/Bundle/RSSClientBundle
```

```
`"require": {
    ...
    "desarrolla2/rss-client-bundle": "dev-master" 
}
`
```

## Actualizando NamesPaces y ficheros de configuración

Algunos NameSpaces han sufrido cambios por lo que descargo la última versión.

```
`mv app/AppKernel.php app/AppKernel_20.php
wget https://raw.github.com/symfony/symfony-standard/master/app/AppKernel.php -P app`
```

Ahora toca copiar y pegar los bundles que tengas activos, en mi caso aproveche, para poner por ejemplo el DoctrineFixturesBundle solo disponible para el entorno de desarrollo.

```
`public function registerBundles()
    {
    $bundles = array(
        ...
        new SFM\WebsiteBundle\SFMWebsiteBundle(),
        new Desarrolla2\Bundle\RSSClientBundle\RSSClientBundle(),
    );

    if (in_array($this->getEnvironment(), array('dev', 'test'))) {
        ...
        $bundles[] =new Symfony\Bundle\DoctrineFixturesBundle\DoctrineFixturesBundle();
        //$bundles[] =new Symfony\Bundle\DoctrineMigrationsBundle\DoctrineMigrationsBundle();
        }

    return $bundles;
    }`
```

El fichero app/autoload.php tambien ha cambiado.

```
`rm app/autoload.php
wget https://raw.github.com/symfony/symfony-standard/master/app/autoload.php -P app`
```

Elimino el contenido de la carpeta vendor y cache.

```
`rm -rf vendor/* app/cache/* app/bootstrap.php.cache
chmod -R 777 app/cache`
```

y ejecuto el instalador.

```
`php composer.phar install`
```

Me da errores, indicando que tengo algunos campos que sobran en el config.yml.

```
[Symfony\Component\DependencyInjection\Exception\InvalidArgumentException] 

  There is no extension able to load the configuration for "nekland_feed" (in /home/dgonzalez/NetBeansProjects/symfony-madrid/app/config/config.yml). Looked for namespace "nekland_feed", found "framework", "security", "twig", "monolog", "swiftmailer", "assetic", "doctrine", "sensio_framework_extra", "jms_aop", "jms_di_extra", "jms_security_extra", "rss_client", "web_profiler", "sensio_distribution"
```

Retiro las entradas referentes a los bundle que he eliminado stfalcon_tinymce y nekland_feed, y continuo obteniendo errores.

```
[RuntimeException]
  The charset setting is deprecated. Just remove it from your configuration file. 

[RuntimeException]                                                                  
  The auto_start setting is deprecated. Just remove it from your configuration file.  

[Symfony\Component\Config\Definition\Exception\InvalidConfigurationException]  
  Unrecognized options "default_locale" under "framework.session" 

[Symfony\Component\Config\Definition\Exception\InvalidConfigurationException]  
  Unrecognized options "secure_controllers" under "jms_security_extra"  
```

Elimino estas entradas tambien y continuo con los errores.

```
[Symfony\Component\Config\Definition\Exception\InvalidConfigurationException]  
  Unrecognized options "users" under "security.providers.in_memory"
```

En este caso solo se trata de cambiar el bloque security tal y como aparece.

```
`security:
    ...
    providers:
        default_provider:
            memory:
                users:
                  user1: { password: pass1, roles: ROLE_ADMIN }`
```

Y listo !!, ya tengo mi sitio listo y con un motor symfony 2.1 bajo el capó.
