---
title: "Instalando un entorno de desarrollo PHP en Ubuntu 10.10"
date: 2011-02-24
tags: [php, ubuntu, linux]
source: https://desarrolla2.com/post/instalando-un-entorno-de-desarrollo-php-en-ubuntu-10-10
---

# [Instalando un entorno de desarrollo PHP en Ubuntu 10.10](https://desarrolla2.com/post/instalando-un-entorno-de-desarrollo-php-en-ubuntu-10-10)

A continuación vamos a ver como instalar un entorno de desarrollo para PHP en Ubuntu 10.10. El objetivo es obtener un equipo de trabajo, lo más cómodo posible y que permita separar los entornos de un proyecto a otro. Además añadiremos algunos paquetes extra que no están directamente relacionados con el desarrollo, pero si con la utilización del equipo. Para la realización de este tutorial, suponemos que se trata de una instalación limpia de ubuntu10.10 en inglés. (nota: intenté probar con una 11.04 pero exploto, creo que aún es pronto ... ). Lo primero, actualizar:

```
sudo apt-get update
sudo apt-get upgrade
```

## Paquetes y programas de ámbito general

Vamos a empezar instalando algunos paquetes que considero imprescindibles, quizá no están directamente relacionadas con el desarrollo en PHP, pero lo mejor es crear un espacio de trabajo amigable que favorezca la productividad.

- **ubuntu-restricted-extras**: Este meta paquete depende de los repositorios multiverse. Al instalar este paquete añadirás soporte mp3, y otros formatos de audio, fuentes de microsoft, JRE, Flash, DVD
- **rar unzip**: Estos paquetes te permitirán trabajar con formatos de compresión muy usados
- **sun-java6-jre sun-java6-plugin**: el SDK oficiales de sun
- **virtualbox-ose**: ¿Alguien no sabe que es virtual box? Es una solución de virtualización de escritorio que te permitirá instalar sistemas Windows, DOS, BSD o Linux como huéspedes
- **gnome-do**: Te permite lanzar fácilmente tus aplicaciones más utilizadas. Contiene multitud de plugins para extender su funcionalidad. A mi concretamente me gusta ponerle el aspecto "Mini". Otro popular lanzador de aplicaciones es synapse, pero no se encuentra en los repositorios oficiales, y como no he notado una diferencia sustancial entre uno y otro me quedo con gnome-do.
- **dia**: Es un editor de diagramas, no acaba de convencerme, pero no conozco otro mejor.
- **jdownloader**:Un gestor de descargas.
- **shutter**:Una herramienta para capturar pantallas.
- **bisigi-themes**:Es un meta paquete con diferentes temas de escritorio para gnome. Bastante mejores que los que vienen por defecto. Instala lo, y selecciona un tema sobrio, con el que te encuentres cómodo, personalmente me gusta "showtime". También recomiendo dejar los efectos de escritorio en bajo, o como mucho en normal.
- **dropbox**: Es un sistema de sincronización de carpetas. Parecido a Ubuntu One, pero bastante mejor. Tiene clientes para todos los sistemas operativos y plataformas. Recomiendo utilizarlo para almacenar todos los archivos importantes y olvidarte de las copias de seguridad. Si no tienes una cuenta, registra te desde este [enlace](http://db.tt/C2UPKAr) y yo ganare unos cuantos megas extra ;)
- **skype**: No me gusta nada el cliente de skype, ni que su protocolo cerrado me impida usar otros clientes, no obstante, está muy estendido en el mundo empresarial, y en ocasiones me toca usar la cuenta de skype para la comunicación con los clientes.

Pongamonos manos a la obra, necesitas añadir algunos repositorios extra:

```
sudo add-apt-repository ppa:shutter/ppa
sudo add-apt-repository ppa:jd-team/jdownloader
sudo add-apt-repository ppa:bisigi/dev
sudo echo "deb http://ppa.launchpad.net/nautilus-dropbox/ppa/ubuntu jaunty main" >> /etc/apt/sources.list
sudo echo "deb-src http://ppa.launchpad.net/nautilus-dropbox/ppa/ubuntu jaunty main" >> /etc/apt/sources.list
sudo apt-get update
```

Ahora instalamos los paquetes:

```
sudo apt-get install -y manpages-es-extra ubuntu-restricted-extras rar unzip sun-java6-jre sun-java6-plugin virtualbox-ose gnome-do jdownloader shutter bisigi-themes nautilus-dropbox skype
```

Configuramos la máquina virtual por defecto:

```
sudo update-alternatives --config java
```

Y elegimos la opción de la máquina virtual propietaria. (/usr/lib/jvm/java-6-sun/jre/bin/java) Una sugerencia es añadir guake (lo instalaremos despues), empathy y quizá rhythmbox a las aplicaciones que se lanzan al inicio, System->Preferences->Startup Aplications ¿No me digas que no las usas siempre? Lo mejor es añadir al inicio las aplicaciones que usas el 90% de las veces que entras en el sistema, ojo no te pases, estás recargando el inicio de sesión.

## Navegadores

En ocasiones he instalado multitud de navegadores extra para probar las aplicaciones en diferentes navegadores y versiones. A la hora de la verdad, no los uso, por lo que instalo uno más y listo.

- **chromium-browser**:la versión libre de Crome, me encanta como funciona!

```
sudo apt-get install -y chromium-browser
```

Firefox es mi navegador por defecto, pero es el que es por sus plugins, yo recomiendo usar los siguientes:

- **[firebug](http://getfirebug.com/)**:La popular herramienta de desarrollo.
- **[fire cookie](https://addons.mozilla.org/en-us/firefox/addon/firecookie/)**:Una extensión de firebug que permite ver y editar las cookies desde el navegador.
- **[lori](https://addons.mozilla.org/en-us/firefox/addon/lori-life-of-request-info/)**:Lori, añade en el pide de página indicadores para el tiempo de carga, peticioes y tamáño de un sitio web, nada que no puedas hacer con firebug, pero más a mano.
- **[page-speed](http://code.google.com/intl/es/speed/page-speed/)**:Permite evaluar y optimizar el rendimiento de tus sitios web, facilitando no solo información sobre errores si no sugerencias sobre como solucionarlos.
- **[tamper data](https://addons.mozilla.org/en-us/firefox/addon/tamper-data/)**:Permite ver y editar peticiones HTTP, muy útil para ver como se comporta una aplicación, o para editar la información que se envía a través del navegador.

He añadido los enlaces a la página de cada uno, para que puedas instalarlos fácilmente, también puedes añadirlos desde Firefox->add-ons->get-add-ons.

## Editores de texto y herramientas de desarrollo

¿Que es un entorno de desarrollo sin IDEs ni editores de texto?

- **guake**: Es una terminal tipo "Quake". Puede mostrarse u ocultarse mediante la tecla F12. Me gusta mas que usar gnome-terminal, por que la tengo disponible en todos los escritorios, y además no "ocupa" espacio en ninguno de ellos. Soporta pestañas.
- **vim**: Añade caracteristicas extra a vi, como por ejemplo moverte a través de los cursores en lugar de las teclas de vi. Estoy tan acostumbrado a usar vim, que no se usar vi :(
- **[netbeans](http://netbeans.org/downloads/)**: ¿Kas naranja o Kas Limon? ¿Eclipse o Netbeans? ... Netbeans. No lo instales desde los repositorios, descargalo de su sitio web.
- **mysql workbench**: Muy útil para crear representaciones gráficas de modelos de datos.
- **[phpmyadmin](http://www.phpmyadmin.net/home_page/index.php)**: La herramienta que más me gusta, para trabajar con mysql.
- **subversion**: Es el sistema de control de versiones que uso, en este aspecto estoy un poco obsoleto, quizá prefieras usar otro.

Con estos y gedit, trabajo perfectamente, no necesito nada más, vamos a instalarlos:

```
sudo apt-get install -y guake vim subversion
```

El resto puedes descargarlos desde los enlaces que facilito. Para poner vim como editor de texto por defecto.

```
sudo update-alternatives --config editor
```

**nota:** Me comentan que phpmyadmin se encuentra disponible en los repositorios, es cierto, pero suelen tener una versión bastante desactualizada. Personalmente me gusta bajarme la última versión y tener opción mas tarde para ajustar la configuración por ejemplo para atacar diferentes bases de datos remotas. En cualquier caso muchos usuarios utilizan el paquete phpmyadmin, ya que tiene como dependencias el resto de paquetes necesarios para un entorno LAMP.

## Servidores

- **mysql**
- **apache**
- **php5**
- **php5-cli**

```
sudo apt-get install -y mysql-server mysql-client apache2 php5 php5-curl php5-gd php5-cli php5-mcrypt php5-mysql php5-xcache php5-xdebug php-doc
```

Activamos un par de módulos de apache.

```
sudo a2enmod vhost_alias
sudo a2enmod rewrite
```

## Configuración del entorno

NetBeans permite crear "grupos" de aplicaciones. Lo mejor es crear un grupo nuevo para cada proyecto, esto me permite tener los conceptos separados. Si dos proyectos son parecidos, o están muy relacionados entre si, mantenlos en el mismo grupo, para poder pasar fácilmente de uno a otro. Para cada proyecto, creo un host, por ejemplo:

```
sudo echo "miproyecto.local 127.0.0.1" >> /etc/hosts
```

Reinicia los navegadores para aplicar la nueva configuración. Ahora para cada proyecto necesitas crear un virtual host. Crea un fichero miproyecto.local en /etc/apache2/sites-available con el siguiente contenido.

```

      ServerName miproyecto.local
      DocumentRoot "/home/ale/NetBeansProjects/miproyecto/trunk"
      DirectoryIndex index.php

        AllowOverride All
        Allow from All

    CustomLog     /var/log/apache2/access.miproyecto.log combined
    ErrorLog      /var/log/apache2/error.miproyecto.log
```

Y para activarlo:

```
sudo a2ensite miproyecto.local
```

Utilizar virtual host para cada entorno de desarrollo te permite tener entornos de desarrollo aislados, y con la misma configuración que tus entornos de producción. Además añado los ficheros de configuración al subversión, por ejemplo en /config/sites/ facilitando la tarea de despliegue.
