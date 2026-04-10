---
title: "Ejecución asíncrona de eventos con el EventDispatcher de symfony"
date: 2019-05-05
tags: [php, symfony, talks]
source: https://desarrolla2.com/post/ejecucion-asincrona-de-eventos-con-el-eventdispatcher-de-symfony
---

Esta es la charla que di en el [evento de php madrid el día 22 de Noviembre de 2017](https://www.meetup.com/phpmad/events/245070402/).

Symfony provee un sistema de eventos para que diferentes componentes puedan comunicarse entre si.  
  

Veremos como pasar de esta ejecución sincrona a una asincrona, y dos ejemplos de implementación de la misma, una básica con mysql y una avanzada con rabbitmq y supervisord.  
  

Finalmente veremos como asegurar esta arquitectura para que sea a prueba de fallos.
