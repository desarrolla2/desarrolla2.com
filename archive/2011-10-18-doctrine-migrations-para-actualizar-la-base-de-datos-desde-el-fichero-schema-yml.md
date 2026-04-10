---
title: "Doctrine migrations para actualizar la base de datos desde el fichero schema.yml"
date: 2011-10-18
tags: [php, symfony1]
source: https://desarrolla2.com/post/doctrine-migrations-para-actualizar-la-base-de-datos-desde-el-fichero-schema-yml
---

# [Doctrine migrations para actualizar la base de datos desde el fichero schema.yml](https://desarrolla2.com/post/doctrine-migrations-para-actualizar-la-base-de-datos-desde-el-fichero-schema-yml)

En este artículo vamos a hablar de doctrine [migrations](http://www.doctrine-project.org/documentation/manual/1_2/zh/migrations), una utilidad de doctrine 1.2 que permite fácilmente actualizar tus bases de datos según las necesidades del proyecto. El primer paso es generar la estructura de carpetas y base de datos necesaria para gestionar las migraciones

```
php symfony doctrine:generate-migration --env=prod proyect_name
```

Veremos que ha creado una tabla en la base de datos, donde almacena la versión de la migración y una carpeta en lib/migration/doctrine Esta instrucción solo es necesario ejecutarlo la primera vez. Después continuaremos a partir de aqui. El siguiente paso, es generar las versiones para la migración para ello ejecutamos el siguiente comando.

```
php symfony doctrine:generate-migrations-diff --env=prod
```

Lo que hace doctrine es comparar la información que contiene el fichero config/doctrine/schema.yml y la información que se encuentra en los modelos lib/model/doctrine/base Puedes comprobar cuales son las diferencias que ha detectado en los ficheros generados lib/migration/doctrine, es muy conveniente revisar cuales son los cambios que piensa aplicar en cada uno de los pasos de la migración. Una vez que estás seguro de que todo está correcto, tan solo indicarle a doctrine que complete el proceso.

```
php symfony doctrine:migrate --env=prod
```

Muy problablemente tengas que ejecutar un build all classes para continuar trabajando, ya que ahora tienes diferencias entre lo que existe en el modelo, y lo que contiene la base de datos

```
php symfony doctrine:build --all-classes
```

Algunas de las ventajas de utilizar migraciones son las siguentes:

- Mantener en el sistema de control de versiones un registro de los cambios realizados en el modelo de datos.
- Capacidad de volver a versiones previas del modelo de datos.

En mi experiencia personal doctrine migration es muy util a la hora de generar pequeños cambios en la base de datos, añadir y quitar campos, claves foreaneas, pero no es la panacea, si el proceso no se completa a veces deja el sistema inestable, por ejemplo cuando falla en la creación de una clave foranea dejando el sistema a medias entre dos versiones. Por lo tanto, te recomiendo probar primero la migración que estás a punto de hacer en una base de datos de desarrollo o test

```
php symfony doctrine:generate-migration --env=prod proyect_name
```
