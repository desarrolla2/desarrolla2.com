---
title: "Desarrollo de extensiones PHP en c/c++"
date: 2013-10-27
tags: [php, talks]
source: https://desarrolla2.com/post/desarrollo-de-extensiones-php-en-c-c
---

# [Desarrollo de extensiones PHP en c/c++](https://desarrolla2.com/post/desarrollo-de-extensiones-php-en-c-c)

Mi presentación en el unconference de Codemotion Madrid en Octubre de 2013. Codemotion es el evento más
  grande dirigido a comunidades de desarrolladores de todo tipo, este año tuvo lugar en la Universidad Politécnica
  de Madrid, y aunque no recuerdo la cifra de asistentes, creo que era entre 2 y 3.000.

Esta charla fue bastante especial, por que es un tema del que no hay demasiada información publicada, y
  practicamente nada en castellano, por lo que creo que aporté un granito de arena a la comunidad dando un poco
  de luz en este asunto.

### Esquema de la charla

1. ¿Que es PHP? “PHP is a serverside scripting language designed for web development but also used as a
    general-purpose programming language”. -wikipedia
  
2. ¿Que es una extensión? “An extension in PHP is in fact a module providing some functionality
    to the PHP Engine.” - Shahar Evron
  
3.  ¿Para que extender PHP?
4. ¿Por que no hacerlo?
5. Ejemplos de código
6.  ¿Que necesito?
    
1. código fuente de php
2. entorno de compilación
3. conocimientos de c/c++
4. conocimientos de php ( bajo el capó )
        
1. SAPI PHP API
2. Signals
3. Sandboxing 
4. LEXER / PARSER
5.  Ejemplos prácticos
    
1. RelaxingCup();
2. PrimeNumbers ->factorize(1000,10000);
3. afile_put_contents();
4. Conclusiones
5. Referencias
