---
title: "Compartir sesiones en symfony"
date: 2011-05-10
tags: [php, symfony1]
source: https://desarrolla2.com/post/compartir-sesiones-en-symfony
---

# [Compartir sesiones en symfony](https://desarrolla2.com/post/compartir-sesiones-en-symfony)

Veamos como compartir la sesión de symfony con otras aplicaciones, en este caso el problema surgio al tratar de implementar la seguridad en CKFinder un gestor de recursos en el servidor. Para ello, según el propio CKFinder, nos indican que debemos implementar una función.

```
/**
 * This function must check the user session to be sure that he/she is
 * authorized to upload and access files in the File Browser.
 *
 * @return boolean
 */
function CheckAuthentication()
{
o// WARNING : DO NOT simply return "true". By doing so, you are allowing
o// "anyone" to upload and list the files in your server. You must implement
o// some kind of session validation here. Even something very simple as...

o// return isset($_SESSION['IsAuthorized']) && $_SESSION['IsAuthorized'];

o// ... where $_SESSION['IsAuthorized'] is set to "true" as soon as the
o// user logs in your system. To be able to use session variables don't
o// forget to add session_start() at the top of this file.

oreturn false;
}

```

Despues de dar algunas vueltas tratando de ver una sesión desde fuera del framework, tratando incluso de extender sfPDOSessionStorage me di cuenta de que la solución era mucho más sencilla, veamos en el ejemplo que aspecto tiene la solución.

```
require_once(dirname(__FILE__) . '/../../../config/ProjectConfiguration.class.php');

function CheckAuthentication() {

    $configuration = ProjectConfiguration::getApplicationConfiguration($app_name, $env, $debug);
    $sf_user = sfContext::createInstance($configuration)->getUser();
    if ($sf_user->hasCredential($credential_name)) {
        return true;
    } else {
        return false;
    }
}

```

Consiste en utilizar el contexto que se crea en el fichero /web/index.php pero sin realizar el dispatch(), para poder acceder al objeto $sf_user, igual que si nos encontraramos en una acción del framework.

Como tarde en caer, y además es algo que no encontré en internet, aqui la dejo, un saludo!
