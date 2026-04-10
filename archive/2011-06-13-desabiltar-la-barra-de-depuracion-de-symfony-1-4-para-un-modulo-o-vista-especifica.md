---
title: "Desabiltar la barra de depuración de symfony 1.4 para un módulo o vista específica."
date: 2011-06-13
tags: [php, symfony1]
source: https://desarrolla2.com/post/desabiltar-la-barra-de-depuracion-de-symfony-1-4-para-un-modulo-o-vista-especifica
---

# [Desabiltar la barra de depuración de symfony 1.4 para un módulo o vista específica.](https://desarrolla2.com/post/desabiltar-la-barra-de-depuracion-de-symfony-1-4-para-un-modulo-o-vista-especifica)

La configuración en cascada de symfony permite que configuremos nuestra aplicación de forma diferente según el entorno, aplicación, módulo o incluso vista, tan solo tenemos que incluir el ficjero YML en la carpeta config donde queremos aplicar la configuración. Puedes encontrar información completa sobre como funciona en el [libro de refrencia de symfony.](http://www.symfony-project.org/reference/1_4/en/03-Configuration-Files-Principles) Normalmente esto es suficiente, pero si estás leyendo esto, ya habrás comprobado que el fichero settings.yml no utiliza la configuración en cascada. Sin embargo la solución es tan sencilla como esto:

```
//my_actions.class.php
    public function preExecute() {
        sfConfig::set('sf_web_debug', false);
        return parent::preExecute();
    }
```

Ya que como ves podemos usar el objeto sfConfig, para modificar la configuración "en caliente" Puedes desactivarlo en todo el módulo como en el ejemplo o solo en las acciones que te interese
