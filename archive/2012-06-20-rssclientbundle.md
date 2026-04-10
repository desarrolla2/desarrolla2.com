---
title: "RSSClientBundle"
date: 2012-06-20
tags: [php, symfony]
source: https://desarrolla2.com/post/rssclientbundle
---

# [RSSClientBundle](https://desarrolla2.com/post/rssclientbundle)

He desarrollado un bundle para symfony2 que consume N servicios de RSS, los ordena por fecha y los retorna en forma de array, que pueden ser fácilmente renderizados en un template. Las páginas del proyecto son:

- [https://github.com/desarrolla2/RSSClientBundle](https://github.com/desarrolla2/RSSClientBundle)
- [http://knpbundles.com/desarrolla2/RSSClientBundle](http://knpbundles.com/desarrolla2/RSSClientBundle)

En estás rutas puedes encontrar la información completa sobre el mismo. No voy a entrar en los detalles de instalación, que ya están en el README de ambos sitios, pero si quiero mostraros lo sencillo de usar que es.

En tu controlador recuperas el servicio y le pides que obtenga los nodos para pasarselos a la vista.

```
`public function indexAction()  
    {  
        $this->client = $this->get('rss_client');  
  
        return array(  
            'feeds'   => $this->client->fetch('channel_name1'),  
        );  
    }  
`
```

En la vista tan solo tienes que recorrer tu array de nodos.

```
`{% block content %}  
    <section>  
        {% for feed in feeds %}              
            <article>  
                <header>  
                    <a href="{{ feed.link }}" target="_blank">{{ feed.title }}</a>  
                    <time>{{ feed.pubDate | date('d/m/Y H:i') }}</time>  
                </header>  
                <p>{{ feed | raw }}</p>  
            </article>        
        {% else %}  
            <p>Not news :(</p>  
        {% endfor %}      
    </section>  
{% endblock %}`
```

Puedes ver un ejemplo del resultado en la [página de noticias](http://symfony-madrid.es/noticias) del grupo de symfony madrid. Espero vuestro feedback!
