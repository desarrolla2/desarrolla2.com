---
title: "Reparando 'perl: warning: Setting locale failed.' en Ubuntu Server"
date: 2011-10-14
tags: [ubuntu, linux]
source: https://desarrolla2.com/post/reparando-perl-warning-setting-locale-failed-en-ubuntu-server
---

# [Reparando "perl: warning: Setting locale failed." en Ubuntu Server](https://desarrolla2.com/post/reparando-perl-warning-setting-locale-failed-en-ubuntu-server)

Un error que me he encontrado en varias ocasiones en instalaciones de Ubuntu Server es el siguiente:

```
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
oLANGUAGE = (unset),
oLC_ALL = (unset),
oLANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_MESSAGES to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory

```

Normalmente en Ubuntu Desktop esto debería reparar el paquete

```
dpkg-reconfigure locales

```

Sin embargo en Ubuntu Server he necesitado utilizar locale-gen para repararlo

```
sudo locale-gen en_US.UTF-8
dpkg-reconfigure locales

```

Supongo que el problema radica en las versiones "customizadas" de Ubuntu Server que proporcionan los proveedores de hosting.
