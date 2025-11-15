+++
title = 'Instrucciones para crear un blog 1'
date = 2024-07-14
categories = ["blog"]
tags = ["Hugo", "blog"]
draft = false
+++

# Empezar un blog completamente gratis. Parte 1
Este es un post especial, porque quiero empezar un blog desde cero usando las herramientas más modernas y actuales.
El objetivo es montar la estructura de blog rápidamente, y que lo montemos sin gastar un euro en servidores usando solo cosas que se encuentran en la comunidad web desde hace años, que además usa la última tecnología, la más rápida y como veremos una de las formas más sencillas de empezar a crear contenido web y hacernos un nombre.

Pues vamos allá, empecemos.

## Cómo hacer el blog con HUGO en github pages
Pues si, esta es para mi la solución más simple y fácil de crear un blog de última generación y **gratis**. Usaremos **HUGO** para la estructura interna de Blog y **gitHub** como servidor, es decir como nuestro almacén de los ficheros y servicio de distribución de la web. *GitHub* es una de las webs y empresas de desarrollo más importantes, y que cualquier programamdor conoce. La usan para distribuir sus proyectos y programar en equipo muchas plataformas y desarrolladores de código libre.

Por otra parte *HUGO* es una plantilla para crear webs genéricas y especialmente indicada par blogs.

## Qué es HUGO?
Hugo es un generador de sitios estáticos que se utiliza para crear blogs y sitios web de manera rápida y eficiente. A diferencia de los sistemas de gestión de contenido (CMS) como WordPress, que generan páginas dinámicamente en un servidor cada vez que un usuario las solicita, *Hugo* precompila todo el contenido en archivos HTML estáticos. Esto hace que los sitios web generados con Hugo sean* extremadamente rápidos*, seguros y fáciles de desplegar en cualquier servidor o servicio de alojamiento estático.

> *Nota:* WordPress genera la página al vuelo consultando una base de datos online, mientras que con HUGO ya está hecha la pagina en html, por eslo es mucho más rápido

### Características clave de Hugo:

 * **Rapidez**: Hugo es uno de los generadores de sitios más rápidos disponibles, capaz de generar miles de páginas en segundos.
 * **Markdown**: Hugo utiliza Markdown para crear contenido, lo que hace que escribir y formatear publicaciones sea simple e intuitivo.
 * **Temas**: Viene con soporte para temas, lo que facilita *cambiar el aspecto del sitio* o reutilizar plantillas de diseño.
 * **Flexible y personalizable**: Hugo permite personalizar plantillas y estructuras de contenido, lo que lo hace adecuado tanto para blogs simples como para sitios web más complejos.
 * **Portabilidad**: Al generar un sitio estático, el contenido puede ser alojado en cualquier lugar, desde servidores tradicionales hasta servicios de almacenamiento en la nube como GitHub Pages o Netlify.
 * **SEO y velocidad optimizados**: Los sitios estáticos generados por Hugo son rápidos y amigables para SEO, lo que ayuda en el rendimiento y posicionamiento en buscadores.

En resumen, **Hugo** es una excelente opción para quienes desean crear blogs o sitios web estáticos.

## Instalación de HUGO
En primer lugar me he instalado HUGO en mi PC usando **winget** de windows, que básicamente consiste en abrir un terminal en cmd y poner esto:

```bash
winget install Hugo.Hugo.Extended
```
Así se ha instalado y ocupa poco unos 30 Megas, mucho menos que *jekyll* que es otro sistema parecido.
Quizás necesites también instalar **Go** que se puede descargar [aquí](https://go.dev/doc/install) simplemente con el ".exe" que descargas.

# Crear tu primer blog
Una vez tenemos HUGO en nuestro PC vamos al directorio donde queremos almacenar y crear el blog **en local** y abrimos un terminal (con *git bash* por ejemplo o con CMD). Boton derecho sobre el explorador de ficheros en dicha carpeta  *abrir en Terminal*, y ya en la ventana del terminal escribe lo siguiente con el nombre que quieras para el blog al final:

```
hugo new site mi_nuevo_blog
```
Esto crea una nueva carpeta en el directorio donde te encontrabas con la estructura básica de HUGO de un blog llamdo "mi_nuevo_blog"

La estructura de ficheros que crea será algo así:

```
mi_nuevo_blog/
├── archetypes/
├── assets/
├── hugo.toml
├── content/
│   ├── _index.md
│   ├── about.md
│   └── posts/
│       ├── post-1.md
│       ├── post-2.md
│       └── post-3.md
├── i18n/
├── data/
├── layouts/
├── static/
│   └── images/
│       └── logo.png
├── themes/
    └── mytheme/
        ├── archetypes/
        ├── assets/
        ├── layouts/
        ├── static/
        ├── theme.toml
        └── README.md

```
Para empezar a trabajar en el blog en local simplemente ve a la carpeta recien creada desde terminal:

`cd mi_nuevo_blog`

## Inicializar GIT
Ya hemos creado la plantilla del blog y nos hemos posicionado con el terminal en dicha carpeta. 
Ahora toca iniciar *git* en local, lo que nos permitirá llevar el registro de todos los cambios en este directorio. Yo siempre tengo instalado git en mi PC en concreto "git bash", por lo que si no lo tienes instalalo antes.
Una vez en el terminal y en la carpeta del nuevo blog ponemos esto:

`git init`

Esto inicia git en la carpeta para el control de cambios y a continuación tenemos que descargarnos una de las plantillas o themes de *HUGO* para nuestro blog. Para esto podemos buscar en github donde hay cientos o en la web de HUGO:

<https://themes.gohugo.io/themes>

Para nuestro caso vamos a elegir una que ya tenemos vista para otro blog y que me gusta usar se llama *Mainroad* y el autor es Vinux (ver [aquí](https://themes.gohugo.io/themes/mainroad/)).
Para copiarla en nuestro blog simplemente copiamos este código y nos hace una copia dentro de la carpeta theme:

`git submodule add https://github.com/Vimux/Mainroad themes/Mainroad`

Si quisieramos otra plantilla como esta que se llama Zen pondríamos esto, de hecho podemos tener varias instaladas e ir cambiando de una a otra, aunque esto es mejor ajustarlo al inicio del blog pues cada una luego tiene sus particularidades:

`git clone https://github.com/frjo/hugo-theme-zen.git themes/zen`
Puedes ver esta plantilla aquí: <https://themes.gohugo.io/themes/hugo-theme-zen/>

Pues ya está copiada la plantilla en la carpeta *themes*, y ahora para ponerla como la oficial de nuestro nuevo blog tenermos que editar el fichero principal e configuración que está en la carpeta raiz y se llama `hugo.toml`.

Lo podemos abrir con cualquier editor de textos y añadir una nueva línea que ponga `theme = Mainroad` o `theme = zen` según el nombre de la plantilla que hayamos elegido:

```
baseURL = 'https://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = Mainroad
```
> NOTA IMPORTANTE: a mi no me gusta hacer lo anterior, pues cada plantilla tiene sus particularidades y es mejor empezar el blog con el ejemplo que el mismo theme da. **Para ello basta con copiar desde la carpeta del theme a la carpeta raiz los ficheros que vengan dentro de la carpeta** *exampleSite*

Es decir, antes de ver el blog de ejemplo vete a la carpeta exampleSite dentro de "themes/zen" y copia todos los ficheros al directorio raiz:

```
mi_nuevo_blog/
├── archetypes/
.
.
├── themes/
    └── zen/
        ├── exampleSite/
            ├── content/
            ├── config.yaml

```
en este caso copiaría la carpeta content completa y el fichero config.yaml al directorio raiz del blog es decir a mi_nuevo_blog/

Ahora si puedo probrar el blog ejecutandpo el siguiente código en *terminal*

`hugo server`

En el terminal dice la dirección para ver la web en algún explorardor como Chrome,Firefox...que será algo así como:

`http://localhost:1313/mi_nuevo_blog/`


## Mi primer post
Ya hemos visto que funciona en local, abrimos firefox y ponemos la url que nos da en terminal y vemos el blog, pues ya lo tenemos listo. Para empezar puedes abrir directamenten los ficheros de ejemplo que hemos copiado y editarlos o crer un nuevo en la carpeta "content/"

Si queremos ir a lo fácil, para crear el primer post escribimos esto en terminal:

`hugo new content content/post/mi-primer-post.md`

Con esto nos crea un fichero llamado *"mi-primer-post.md"* en el directorio *post* dentro del directorio *content* que es donde debemos ponerlos.

Ahora en cualquier editor de texto escribiremos texto en *markdown* y automáticamente lo veremos en el explorador web en la dirección local `http://localhost:1313/mi_nuevo_blog/`. Podemo editarlo y verlo al mismo tiempo si ponemos las ventanas en paralelo, aunque yo por ejemplo suelo usar Visual Studio para esto, pero es tan simple que no haría falta.

Para parar el servidor basta con pulsar *control + c* en terminal.

### Otra forma de crear un post
Si ya tienes algun post escrito quizás e smás fácil directamente copiar y pegar el fichero de plantilla de post, o el anterior, y ya en el editor de texto modificarlo.
Una buena opción es dejar una plantilla hecha con el formato de post básico y definirla en el encabezado como _draft = true_. Así siempore lo tendremos en la carpeta post disponible y basta con copiarlo, pegarlo y cambiarle de nombre para el nuevo post. 

Para los nombres yo particularmente prefiero nombrarlos con la fecha por delante así quedan ordenados por defecto por nombre en el directorio. 

## Formato de un post

Como puedes ver abriendo cualquier de post de ejemplo, todos los ficheros empiezan con un encabezado YAML que va entre unas líneas `---` o entre `+++`:

```yaml
---
title: "Configurpost del viernesation"
author: "Fernando Dudo"
draft: true
---

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. 

```

En estos encabezados se ponen muchas caractetisticas del post que iremos viendo, pero no salen en el texto de la web

Por ejemplo si pone la linea de _draft=true_  el post no se publica ya que queda como borrador.

Aunque puedes servir en local los borradores para verlos antes de publicarlos con este comando:

`hugo server -D`


Por hoy esto es suficiente, seguimos en próximos artículos.
