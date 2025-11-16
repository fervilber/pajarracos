+++
title = 'Hugo Shortcodes'
date = 2025-11-16
categories = ["Blog"]
tags = ["Hugo", "Blog"]
draft = false
toc   = false   # ← desactiva el TOC
author = "Sofi"
author_bio = "Genia del tiempo"
author_avatar = "img/avatar_sof01.png"
+++

![](../../img/clip_2025-11-17-00-41-33_2025-11-16_shortcodes.png)

# Shortcodes, mejorando el Blog
En el anterior post en el que expliqué cómo hacer un botón de pago y cobro en BTC desde el blog tuve bastantes problemas para hacer un iframe o ventana desde el mismo texto del blog.

Al final, estoy escribiendo un blog estático con HUGO, y por medidas de seguridad los parámetros normales no dejan ejecutar ni javascript ni código HTML que pueda ser potencialmente peligroso. Y para solucionar estas cosas me enteré indirectamente de los shortcodes y de las ventajs que ofrecen de modularidad y estandarización para la programación y escritura de este mismo blog.

# ¿qué son los shortcodes?
Los shortcodes de Hugo son fragmentos de código reutilizables que puedes insertar dentro del contenido de tus páginas (normalmente en archivos Markdown) para generar HTML más complejo sin tener que escribirlo a mano. 
Se trata de hacer plantillas de código, en los que dejas ciertas partes como parámetros, con lo que si vas a repetir este código para diferentes usos puedes ahorrar mucho tiempo.


# ¿Cómo funcionan?

**Definición** – Un shortcode es un archivo .html (o .md) que se coloca en el directorio `layouts/shortcodes/` de tu sitio.

**Uso** – Dentro del contenido lo llamas con la sintaxis  &#123;&#123;< nombre >&#125;&#125; o &#123;&#123;% nombre % &#125;&#125;.
 * &#123;&#123;< … >&#125;&#125; escapa automáticamente cualquier HTML que incluya el shortcode.
 * &#123;&#123;% … %&#125;&#125; permite que el HTML sea interpretado tal cual (útil cuando el shortcode devuelve etiquetas `<script>`, `<iframe>`, etc.).

 **Parámetros** – Puedes pasarle argumentos y/o contenido interno:

&#123;&#123;< youtube id="dQw4w9WgXcQ" >&#125;&#125;

o con contenido interno:

&#123;&#123;% alert %& &#125;&#125;
Este es un mensaje importante.
&#123;&#123;% /alert %&#125;&#125;

## Ventajas

**Reutilización**: Evitas repetir bloques de HTML o lógica en cada página.

**Mantenimiento**: Cambias la presentación en un solo archivo y se actualiza en todo el sitio.

**Flexibilidad**: Los shortcodes pueden recibir parámetros, leer variables del front‑matter, e incluso ejecutar lógica Go‑templates para generar contenido dinámico (por ejemplo, galerías de imágenes, incrustaciones de videos, tablas de precios, etc.).

**Seguridad**: Al usar la variante &#123;&#123;< … >&#125;&#125; el contenido queda escapado, reduciendo riesgos de inyección accidental.

## Ejemplo práctico

Supongamos que quieres incrustar videos de YouTube de forma recurrente en el blog, entonces creas en *layouts/shortcodes/youtube.html* este fichero:

```js
{{ $id := .Get "id" }}
<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/{{ $id }}" frameborder="0" allowfullscreen></iframe>
</div>
```
Luego, en cualquier artículo Markdown bastaría con hacer esto:

 &#123;&#123;< youtube id="dQw4w9WgXcQ" >&#125;&#125; 

Hugo renderizará el HTML definido en el shortcode, insertando el iframe con el ID proporcionado.

# Resumen

Los shortcodes son una herramienta poderosa de Hugo que te permite encapsular fragmentos de plantilla y reutilizarlos fácilmente dentro del contenido, manteniendo tu sitio limpio, modular y fácil de mantener.

# Ejemplos en este blog
Una vez que me enteré de lo que son los shortcode los he usado de momento para 2 cosas:

 1. Plantilla de botón
 2. Insertar video de youtube

Son dos ejemplos típicos pero que de verdad me han supuesto una gran utilidad. Lo primero ha sido crear una carpeta nueva en el root del proyecto llamada `layouts/shorcodes` donde vamos a guardar los ficheros de configuración de cada nuevo shortcode que hagamos.

El primer caso el de insertar un botón el fichero se llama `payment-button.html` y el contenido del mismo es :

```html
<button onclick="window.open('{{ .Get "url" }}', '_blank', 'width=800,height=600,scrollbars=yes,resizable=yes')" style="padding: 12px 24px; font-size: 16px; background-color: #007bff; color: white; border: none; border-radius: 5px; cursor: pointer; font-weight: bold;">
  {{ .Get "text" | default "Pagar con Flash" }}
</button>
```
Cada vez que quiera insertar un botón de pago en el blog, lo único que tengo que hacer es poner en url el link del pago que me da paywithflash así y el texto que aparecerá en el boton

 &#123;&#123;< payment-button url="https://app.paywithflash.com/pay/User?flashId=3243" text="Compra mi libro" >&#125;&#125; 

{{< payment-button url="https://app.paywithflash.com/pay/User?flashId=3243" text="Compra mi libro" >}}


Como ves es una manera mucho más sencilla y eficiente de hacerlo.

## Insertar video de youtube

Para inserter un video de youtube hemos realizado una solucion parecida. Guardando un fichero llamado 'iframe.html` cuyo contenido es este:

```html

<iframe 
  width="{{ .Get "width" | default "560" }}" 
  height="{{ .Get "height" | default "315" }}" 
  src="{{ .Get "src" }}" 
  title="{{ .Get "title" | default "Embedded content" }}"
  frameborder="{{ .Get "frameborder" | default "0" }}"
  allow="{{ .Get "allow" | default "accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" }}"
  referrerpolicy="{{ .Get "referrerpolicy" | default "strict-origin-when-cross-origin" }}"
  {{ if .Get "allowfullscreen" }}allowfullscreen{{ end }}
  style="{{ .Get "style" }}">
</iframe>

```

Cuando quiera insertar un video lo único que tengo que hacer ahora es meter esta simple linea de código:

 &#123;&#123;< iframe src="..." >&#125;&#125;

 y se insertará el video.

incluso si quisiera especificar las medidas lo puedo hacer de manera más simple así:

 &#123;&#123;< iframe src="https://www.youtube.com/embed/VIDEO_ID" width="800" height="450" allowfullscreen="true" >&#125;&#125;


 Bueno y eso es todo, cada día aprendemos una cosa nueva, Un saludo!!
