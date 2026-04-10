---
title: "mysql error 1025 (HY000): Error on rename of './foo' (errorno: 150)"
date: 2011-05-30
tags: [mysql]
source: https://desarrolla2.com/post/mysql-error-1025-hy000-error-on-rename-of-foo-errorno-150
---

# [mysql error 1025 (HY000): Error on rename of './foo' (errorno: 150)](https://desarrolla2.com/post/mysql-error-1025-hy000-error-on-rename-of-foo-errorno-150)

A veces cuando tratamos de eliminar registros o tablas en mysql nos encontramos con este error, mysql no puede continuar, por que quedaría alguna clave foranea rota. No se muy bien por que a veces no te deja eliminar la clave foranea, pero veamos a continuación una forma sencilla de resolver el problema.

primero ejecutamos:

```
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='TRADITIONAL';

```

Despues continuamos ejecuntando las sentencias necesarias, y finalmente dejamos la cosa como estaba

```
SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

```
