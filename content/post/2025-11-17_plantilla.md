+++
title = 'Hacer que varios autores firmen el Blog'
date = 2025-11-17
categories = ["Blog"]
tags = ["Hugo", "Blog"]
draft = false
toc   = false   # ← desactiva el TOC
author = "Evi"
author_bio = "Super gril from Zimbwe"
author_avatar = "people/person02.png"
thumbnail = 'img/clip_2025-11-17-20-02-36_2025-11-17_plantilla.png'
+++

![](../../img/clip_2025-11-17-20-02-36_2025-11-17_plantilla.png)


Esto de programar con CURSOR es una maravilla para los que no sabemos programar, jaja!!. Digamos que algo de idea tengo, pero antes tardaba mucho en solucionar cosas que ahora con la IA tardo muy poco, y encima me lo soluciona bien.

El último ejemplo que he probado es añadir a esta plantilla de Blog la posibilidad de que un autor diferente firme un post. 

## El problema
Llevo años usando HUGO para mis blogs, desde que lo probé me parece fantástico, no solo por la enorme cantidad de plantillas que tiene, y de que cada vez que lo miro hay más, sino también por la estructura simple, sencilla y ordenada de todo. Una vez que te orientas en el ecosistema de blogs con HUGO o con JellyFish que vienen a ser parecidos, es difícil cambiar.

Pero como cada plantilla es un mundo, dentro del orden general, hay cosas que cuesta cambiar, al menos antes. Por ejemplo en la plantilla o theme de este blog llamada Mainroad no permite la firma de los post por diferentes usuarios. He intentado muchas veces arreglarlo yo con mis escasos conocimientos de HUGO, pero nada nunca lo había conseguido.

Ayer me puse en CURSOR , se lo dije a la IA y en 1 minuto me cambió las dos cosas que dice que hacía falta, y olé!!, ya está todo funciona bien y puedo añadir a nuevos autores de manera sencilla, simplemente desde el encabezado añadiendo su nombre, bio y avatar.

Simplemente he añadido esto al YAML que se pone en cada post arriba y listo

```bash
author = "Evi"
author_bio = "Super gril"
author_avatar = "img/people/evita.png"

```
Y mira que lo había intentado veces antes, y la IA en un minuto.

## Lo que ha hecho la IA

El tema Mainroad solo usa el autor global definido en `hugo.toml` y para ampliar los autores ha pensado en hacer esto: modificar los templates para que:

 * Primero busquen `.Params.author` (parámetros del post), y si no existe, 
 * usen `.Site.Author` (autor global por defecto)

Para eso ha cambiado dos archivos:

 1. `themes/Mainroad/layouts/partials/post_meta/author.html` para cambiar de `.Site.Author.name` a `.Params.author` 
 2. `themes/Mainroad/layouts/partials/authorbox.html` para cambiar `.Site.Author.` a `.Params.author` 


