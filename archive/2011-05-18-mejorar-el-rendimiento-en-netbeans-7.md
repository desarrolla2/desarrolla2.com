---
title: "Mejorar el rendimiento en Netbeans 7"
date: 2011-05-18
tags: [php]
source: https://desarrolla2.com/post/mejorar-el-rendimiento-en-netbeans-7
---

# [Mejorar el rendimiento en Netbeans 7](https://desarrolla2.com/post/mejorar-el-rendimiento-en-netbeans-7)

ATENCIÓN: Este artículo es de mayo de 2011, y está no está actualizado, lee los comentarios.

En la actualidad utilizo como entorno de desarrollo para PHP ubuntu10.10 + netbeans7 con OpenJdk para la maquina virtual. Uno de los problemas a los que me enfrento es que la tarea de "scanning projects" era muy pesada, creo que trabajar con un portatil con un disco duro no muy eficiente ( 4800rmp ) agrava el problema. Mientras se ejecuta el proceso, el sistema se pone más lento, y aumenta la carga del sistema. La solución la proporciona un plugin para netbeans, que ofrece que este proceso no se ejecute constantemente sino solamente cuando lo necesitas. Además en netbeans7 desaparecio el menú contextual "refresh folder" muy util cuando netbeans no reflejaba algunos cambios realizados fuera del IDE. Para añadir el plugin tan solo sigue los siguientes pasos

- Tools -> Plugins
- Settings->Add
- Añadimos la siguente URL [http://deadlock.netbeans.org/hudson/job/nbms-and-javadoc/lastStableBuild/artifact/nbbuild/nbms/updates.xml.gz](http://deadlock.netbeans.org/hudson/job/nbms-and-javadoc/lastStableBuild/artifact/nbbuild/nbms/updates.xml.gz)
- Updates->Reload Catalog
- Available Plugins
- Buscamos el plugin "SCAN ON DEMAND" y lo instalamos
- Reiniciamos el IDE y listo!

Tan sencillo como esto. Hemos mejorado considerablemente el consumo de recursos y de memoria en Netbenas7 sobre ubuntu, aunque supongo que funcionará igual en cualquier plataforma.
