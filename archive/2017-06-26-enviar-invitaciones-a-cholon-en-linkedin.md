---
title: "Cómo enviar invitaciones en Linkedin de manera masiva"
date: 2017-06-26
tags: [linkedin, spam]
source: https://desarrolla2.com/post/enviar-invitaciones-a-cholon-en-linkedin
---

Estoy incrementando mi uso de la red social **Linkedin**. Tal y como os comentaba en mi anterior post
  sobre [**el spam en esta red y cómo los
    recruiters contactan conmigo de una manera cuanto menos desafortunada**](https://desarrolla2.com/post/el-spam-en-linkedin), quiero aumentar mi red de
  contactos para que a su vez esto suponga una mejora del tráfico a mi página web.

Un cliente me aseguró tener nada menos que 30.000 contactos y, gracias a ellos, ha conseguido incrementar las
  visitas a su página web con la publicación de sus artículos en esta red social.

Tras analizar cómo Linkedin me puede ayudar en este cometido, me encuentro en la tesitura de aumentar mi red
  de contactos. No quiero enviar invitaciones de forma masiva aunque sí en gran cantidad, ya que corre el rumor
  de que te pueden bloquear la cuenta si detectan comportamientos anómalos.

Para poder enviar invitaciones a cierta cantidad de contactos he elaborado un pequeño script. Esto me permite
  mandar invitaciones a todos los contactos de la página "My network".

![Enviar imágenes "a cholón" en linkedin](https://admin.desarrolla2.com/uploads/3e7070264362ea2e33dca178a0a2ccd4.png")

Enviar imágenes "a cholón" en linkedin

Una vez en está página despliega las herramientas de desarrollador de tu navegador y ejecuta el
  siguiente código.

```
`var sendInvitations;

(sendInvitations = function () {
  var maximumSentInvitations = 100;
  var currentSentInvitations = 0;
  var maximumTimeBetweenClicks = 15;
  var buttonQuery = '[data-control-name="invite"]';

  function sendInvitation() {
    if (currentSentInvitations >= maximumSentInvitations) {
      console.log('all invitations sent');
      return;
    }
    var $buttons = $(buttonQuery);
    if ($buttons.length <= 0) {
      clearInterval(timeout);
      return;
    }
    var key = Math.floor(Math.random() * $buttons.length);
    var $button = $buttons[key];
    if (!$button) {
      return;
    }
    $button.click();

    currentSentInvitations++;
    console.log('invitation ' + currentSentInvitations + ' sent');
    setTimeout(sendInvitation, maximumTimeBetweenClicks * 1000 * Math.random());
  }

  function moveWindow() {
    if (currentSentInvitations >= maximumSentInvitations) {
      return;
    }
    var offset = Math.floor(Math.random() * document.body.scrollHeight);
    $('html, body').animate({scrollTop: offset}, 'slow');
    setTimeout(moveWindow, maximumTimeBetweenClicks * 1000 * Math.random() * 4);
    console.log('moved window');
  }

  sendInvitation();
  moveWindow();
})();`
```

Lo que hace este pequeño script es enviar una invitación cada cierto tiempo, a uno de los contactos de
  forma aleatoria. Luego mueve la página a una posición aleatoria, lo que hace, dependiendo la posición
  en la que caiga, que se añadan más contactos a la lista.

Finalmente cuando alcanza un límite de invitaciones enviadas, se detiene.

Aprovecho estas líneas para pedir perdón a todos por el “spam”, ya no sólo por la
  cantidad de solicitudes que he generado sino por las que pueda llegar a contribuir de una u otra forma con la
  elaboración de este post.
