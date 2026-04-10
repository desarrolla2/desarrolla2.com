---
title: "Instalar la última versión del JDK y JRE de Java / Oracle en Fedora 13"
date: 2013-02-22
tags: [linux, java]
source: https://desarrolla2.com/post/instalar-la-ultima-version-del-jdk-y-jre-de-java-oracle-en-fedora-13
---

# [Instalar la última versión del JDK y JRE de Java / Oracle en Fedora 13](https://desarrolla2.com/post/instalar-la-ultima-version-del-jdk-y-jre-de-java-oracle-en-fedora-13)

Si quieres instalar la última versión del JDK y del JRE en tu equipo con fedora, sigue estos pasos.

## Descarga los RPM de la web de Java

En esta web tienes los enlaces de[ descarga para el JDK y el JRE](http://www.oracle.com/technetwork/java/javase/downloads/index.html), si no sabes lo que es el JDK y el JRE son dos paquetes, el primero contiene las herramientas necesarias para desarrollar software en tecnología java, y el segundo contiene la Java Virtual Machine, necesaria para ejecutar software en Java.

Normalmente huiría del uso del software original de Oracle, por su politica de empresa, tan "fea" con el Software Libre, pero en mi caso particular, tengo que ejecutar ciertas librerías que no acaban de funcionar correctamente, en Linux y con el OpenJDK.

Volviendo al asunto.

 

## Instala los binarios

Ejecuta lo siguiente.

```
sudo yum install jdk-7u15-linux-x64.rpm -y
sudo yum install jre-7u15-linux-x64.rpm -y
```

Por lo visto la versión 7.15 genera unos mensajes de error, por la forma en la que está empaquetado, pero no afectarán a la ejecución normal del entorno.

```
Unpacking JAR files...
  rt.jar...
Error: Could not open input file: /usr/java/jre1.7.0_15/lib/rt.pack
  jsse.jar...
Error: Could not open input file: /usr/java/jre1.7.0_15/lib/jsse.pack
  charsets.jar...
Error: Could not open input file: /usr/java/jre1.7.0_15/lib/charsets.pack
  localedata.jar...
Error: Could not open input file: /usr/java/jre1.7.0_15/lib/ext/localedata.pack
  Verifying  : jre-1.7.0_15-fcs.x86_64
```

 

## Añadir los binarios de Java al PATH del sistema

Yo añadi las siguientes lineas al final del fichero /etc/profile

```
export PATH=/usr/java/default/bin:$PATH
export JAVA_HOME=/usr/java/default
```

Es necesario salir de la sesion del terminal y entrar de nuevo, para que las variables de entorno sean actualizadas.

 Listo, ya tienes tu nueva versión de java funcionando como un tiro
