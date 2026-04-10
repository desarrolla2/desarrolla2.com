---
title: "edit.:sf_format' route has some missing mandatory parameters"
date: 2011-07-08
tags: [php, symfony1]
source: https://desarrolla2.com/post/edit-sf-format-route-has-some-missing-mandatory-parameters
---

# [edit.:sf_format" route has some missing mandatory parameters](https://desarrolla2.com/post/edit-sf-format-route-has-some-missing-mandatory-parameters)

Este error se produce al sobre escribir el método save() de un formulario Doctrine.

```
    public function save($con = null) {
        parent::save($con);
        // your code here!
    }

```

Normalmente esto no tendría por que producir ningún tipo de error, pero sin embargo el admin generator de Doctrine utiliza la respuesta de $form->save() de forma parecida a esto:

```
protected function processForm(sfWebRequest $request, sfForm $form) {
        // ..
        // blah, blah, blah, code
        
        $my_object = $form->save();
        
        // ..
        // blah, blah, blah, code

        $this->redirect(array('sf_route' => 'my_route', 'sf_subject' => $my_object));
    }

```

El código de esta ejemplo está muy recortado, ya que no tiene sentido que muestre el código completo, que se genera dependiendo de las opciones que tengas seleccionadas en tu generator.yml, lo que quiero que veas, es que espera que regreses algo en el $form->save(); entonces tan solo tenemos que modificar nuestro metodo save() de la siguiente forma para que todo funcione de nuevo correctamente.

```
    public function save($con = null) {
        $return = parent::save($con);
        // your code here!
        
        return $return;
    }

```
