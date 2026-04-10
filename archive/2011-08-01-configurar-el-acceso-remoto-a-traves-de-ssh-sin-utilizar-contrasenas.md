---
title: "Configurar el acceso remoto a través de ssh sin utilizar contraseñas"
date: 2011-08-01
tags: [linux, bash]
source: https://desarrolla2.com/post/configurar-el-acceso-remoto-a-traves-de-ssh-sin-utilizar-contrasenas
---

# [Configurar el acceso remoto a través de ssh sin utilizar contraseñas](https://desarrolla2.com/post/configurar-el-acceso-remoto-a-traves-de-ssh-sin-utilizar-contrasenas)

[OpenSSH](http://en.wikipedia.org/wiki/OpenSSH) es un conjunto de herramientas que permiten la comunicación cifrada en una red de computadoras a través del protocolo [SSH](http://en.wikipedia.org/wiki/Secure_Shell) OpenSSH es una libre y se encuentra instalado por defecto en todas las distribuciones linux que conozco.

## La clave pública

La comunicación a través de SSH se basa en un par de claves relacionadas pero no deducibles entre sí. Una de ellas es pública y para iniciar la comunicación cada uno de los interlocutores necesita tener la clave pública del otro. El protocolo OpenSSH se encarga de intercambiar estas claves antes de iniciar una comunicación si no dispone de ellas. Te sonará ver un mensaje similar a este

```
ssh desarrolla2.com
The authenticity of host 'desarrolla2.com (123.123.123.123)' can't be established.
RSA key fingerprint is 6e:59:7c:f5:18:bf:71:4a:77:5c:79:2b:91:a8:99:d9.
Are you sure you want to continue connecting (yes/no)?
```

Si aceptas tu equipo almacenará la clave publica del equipo con el que tratas de conectar. Las claves y los hashes dominios conocidos se guardan en tu directorio .ssh

```
cat ~/.ssh/known_hosts
```

Tu clave publica se encuentra en tu directorio .ssh

```
cat ~/.ssh/id_rsa.pub
```

## La clave privada

Se encuentra en tu directorio .ssh

```
cat ~/.ssh/id_rsa
```

Esta clave es secreta y si esta está comprometida, toda la seguiridad del sistema están comprometidas. ( por ejemplo si enviaste por correo electronico sin cifrar ). Si tu sistema no genero por defecto un par de claves, ejecuta lo siguiente

```
ssh-keygen -b 4096 -t rsa
```

## Copiando tu clave pública

Con el siguiente comando puedes copiar tu clave pública en un servidor remoto

```
ssh-copy-id root@desarrolla2.com
```

Lo que hace este comando es copiar tu clave publica dentro del fichero authorized_keys del servidor

```
cat ~/.ssh/authorized_keys
```

A partir de ahora no necesitaras tener que escribir de nuevo la contraseña en este servidor.
