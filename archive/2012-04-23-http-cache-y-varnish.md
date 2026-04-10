---
title: "HTTP Caché y Varnish"
date: 2012-04-23
tags: [varnish, cache, http-cache, talks]
source: https://desarrolla2.com/post/http-cache-y-varnish
---

# [HTTP Caché y Varnish](https://desarrolla2.com/post/http-cache-y-varnish)

A continuación segunda presentación del grupo de trabajo Symfony Madrid en Abril de 2012.

### Esquema

1. ¿Por qué?
    
1. GWT: Asegura que mi sitio es lento
2. Es (posiblemente) el sistema de cache quemás eficaz y fácil de implementar.
3.  Objetivo
    
1. Nunca generar lamisma respuesta dos veces.
2. Es un protocolo de cache que debería ser respetado por todas las capas intermedias y por los
        navegadores.
      
3. Freshness
    
1. En la mayoría de los casos es la mejor estrategia de cache.
2. Validation
    
1. Cuando no puedes predecir la frecuencia con la que cambian tus recursos.
2. ESI
3. Estandar poco claro
4. Problemas para invalidar la caché.
5. Varnish HTTP
    
1. Proxy Cache
2. Fail Over System Load Balancer
3. Obedece cabeceras HTTP Cache Cache de recursos estáticos Modifica las cabeceras y las cookies de
        peticiones y respuestas.
      
4. Varnish Configuration Language
5. Conclusiónes
    
1. HTTP Cache + Varnish por si solo son insuficiente, necesitas utilizar otras técnicas, como carga
        asincrona de recursos, minimización, ect...
